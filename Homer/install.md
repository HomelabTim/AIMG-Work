```bash
docker run -d -p 8080:8080 --name homer -v /homer/assets:/www/assets --restart=always ghcr.io/georgegedox/homergx:latest
```