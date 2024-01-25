# code to install SigNoz
```bash
git clone -b main https://github.com/SigNoz/signoz.git && cd signoz/deploy/
```
```bash
docker-compose -f docker/clickhouse-setup/docker-compose.yaml up -d
```
```bash
docker ps
```