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

```bash
sudo hostnamectl set-hostname (type)-(service)-(number) #ex. Stage-BlazorServer-1
```
```bash
sudo nano /etc/hosts
```
- reboot the server to see changes
```bash
sudo reboot
```

## Github Runner Installation

- Go to: `Repo > Settings > Actions > Runners` in GitHub  

- Click the green `New self-hosted runner` button

![Step1](gh-actions/img/pic.png)

- Click `Linux`

![Step2](gh-actions/img/pic2.png)

- Go to the VM

- Login as `user` if not already

>[!CAUTION]
>After logging in as `user` run every command in the `Download` section

## Configuring The Runner

>[!TIP]
>Run the first command in the configure section

- For name of the runner group, hit Enter so `Default` is used

- For name of the runner, use the following template: `gh-runner-(type)-(service)-(number)`

- For additional labels, enter: `staging` or `production`,`server(number)`

- For name of the work folder, hit Enter so `_work` is used

- Start the runner:
```bash
./run.sh 
```
- wait for the github runner to update

- After it is updated press `ctrl+c`

- Run these commands to run on boot:
```bash
sudo ./svc.sh install root
```
```bash
sudo ./svc.sh start
```
>[TIP]
>Make sure to update your github action with the new runner!