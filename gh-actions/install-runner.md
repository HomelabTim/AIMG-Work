# Install GitHub Runner

## Change IP/Hostname

- Login as `user` if not already
- Change the `IP` of the server
```bash
sudo nano /etc/network/interfaces
```
- Change the `Hostname`
>[!TIP]
>For the name go with a convention like this: 
>type = [Stage,Prod]
>service = [APISever,BlazorServer]
>number = [1,2,3]
>example = Stage-BlazorServer-1

```bash
sudo hostnamectl set-hostname type-service-number
```
sudo nano /etc/hosts

- reboot the server to see changes
```bash
sudo reboot
```

## API Github Runner Installation

Go to: https://github.com/aimg1/aimg-backend/settings/actions/runners  
Click the green ‘New self-hosted runner’ button
Click ‘Linux’

VM
Login as ‘user’ if not already
Cd
cd ./actions-runner
Back on github, run the ./config.sh command under ‘Configure’

For name of the runner group, hit Enter so ‘Default’ is used
For name of the runner, use the following template:gh-runner-<staging/prod>-api-<server #>

For additional labels, enter: aimg-backend,<staging/production>,api-<server # used above>
API Ex: 

For name of the work folder, hit Enter so ‘_work’ is used

./run.sh - wait for the github runner to update
ctrl+c
sudo ./svc.sh install root
sudo ./svc.sh start

Update Github Action
Open up the CI/CD workflow file (.github/workflows/main.yml) 
https://github.com/aimg1/aimg-backend/blob/main/.github/workflows/cicd-api.yml 
 Append the api-<server #> label that was used above to the list below
Commit and push the change
Done! Any deploy to prod will now be pushed to the new server as well
