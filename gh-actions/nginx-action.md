# Nginx github action
run this in a github action to deploy a nginx website to a self-hosted machine. And rememeber to configure the github runner. 

```yaml
name: MyWebsite CI/CD

on:
  push:
    branches:
      - staging
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Create gh-actions directory
        run: |
          mkdir -p gh-actions
          ls -l  # Debugging step: List contents of the current directory
          ls -l gh-actions  # Debugging step: List contents of gh-actions directory

      - name: Copy files to gh-actions directory
        run: |
          find . -mindepth 1 -maxdepth 1 -not -name 'gh-actions' -exec cp -r {} gh-actions/ \;

      - name: Upload build artifact
        uses: actions/upload-artifact@v3
        with:
          name: dist
          path: gh-actions

      - name: Add nginx config to build artifact
        uses: actions/upload-artifact@v3
        with:
          name: nginx
          path: nginx.conf

  deploy-staging:
    if: github.event.ref == 'refs/heads/staging'
    environment: STAGING
    runs-on: [self-hosted, homelabtim, staging]
    needs: build
    env:
      DOMAIN: ${{ vars.DOMAIN }}
      PORT: ${{ vars.PORT }}
    steps:
      - name: Cleanup build folder
        run: |
          ls -la ./
          rm -rf ./* || true
          rm -rf ./.??* || true
          ls -la ./

      - name: Download build artifact
        uses: actions/download-artifact@v3
        with:
          name: dist

      - name: Download NGINX config
        uses: actions/download-artifact@v3
        with:
          name: nginx

      - name: Set NGINX config
        run: |
          sed -i "s/{{DOMAIN}}/$DOMAIN/" nginx.conf
          sed -i "s/{{PORT}}/$PORT/" nginx.conf

          cat nginx.conf > /etc/nginx/sites-available/$DOMAIN

          if [ ! -L /etc/nginx/sites-enabled/$DOMAIN ] ; then
            ln -s /etc/nginx/sites-available/$DOMAIN /etc/nginx/sites-enabled/
          fi

      - name: Hot Reload NGINX
        run: /etc/init.d/nginx reload

      - name: Deploy to NGINX
        run: |
          rm -rf /var/www/$DOMAIN
          cp -r ./ /var/www/$DOMAIN

  deploy-production:
    if: github.event.ref == 'refs/heads/main'
    environment: PRODUCTION
    runs-on: [self-hosted, homelabtim, production, "${{ matrix.runner }}"]
    strategy:
      max-parallel: 1
      matrix:
        runner: [server-1]
    needs: build
    env:
      DOMAIN: ${{ vars.DOMAIN }}
      PORT: ${{ vars.PORT }}
    steps:
      - name: Cleanup build folder
        run: |
          ls -la ./
          rm -rf ./* || true
          rm -rf ./.??* || true
          ls -la ./

      - name: Download build artifact
        uses: actions/download-artifact@v3
        with:
          name: dist

      - name: Download NGINX config
        uses: actions/download-artifact@v3
        with:
          name: nginx

      - name: Set NGINX config
        run: |
          sed -i "s/{{DOMAIN}}/$DOMAIN/" nginx.conf
          sed -i "s/{{PORT}}/$PORT/" nginx.conf

          cat nginx.conf > /etc/nginx/sites-available/$DOMAIN

          if [ ! -L /etc/nginx/sites-enabled/$DOMAIN ] ; then
            ln -s /etc/nginx/sites-available/$DOMAIN /etc/nginx/sites-enabled/
          fi

      - name: Hot Reload NGINX
        run: /etc/init.d/nginx reload

      - name: Deploy to NGINX
        run: |
          rm -rf /var/www/$DOMAIN
          cp -r ./ /var/www/$DOMAIN
```
