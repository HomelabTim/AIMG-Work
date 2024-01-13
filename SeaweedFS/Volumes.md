# First Create The Proper Directory

- Add `swdata` to the end of the directory of your choice ex:

```bash
sudo mkdir /mnt/pve/local-xfs-1/swdata
```
# Initialize The Volume

- You can run the following line from anywhere to connect a volume to a master.

`weed volume -dir=/path/to/your/directory -mserver=**Server IP**:9333 -port=8001 -ip=**IP of your volumes server** -dataCenter=**Name of volume server**`

ex:
```bash
weed volume -dir=/mnt/pve/local-xfs-1/swdata -mserver=10.2.17.25:9333 -port=8001 -ip=10.2.1.4 -dataCenter=H11VS1
```

## Running More Than One Volume Directory

```bash
weed volume -dir=/mnt/pve/local-xfs-1/swdata -mserver=10.2.17.25:9333 -port=8001 -ip=10.2.1.4 -dataCenter=H11VS1

weed volume -dir=/mnt/pve/local-xfs-2/swdata -mserver=10.2.17.25:9333 -port=8001 -ip=10.2.1.4 -dataCenter=H11VS1

weed volume -dir=/mnt/pve/local-xfs-3/swdata -mserver=10.2.17.25:9333 -port=8001 -ip=10.2.1.4 -dataCenter=H11VS1

weed volume -dir=/mnt/pve/local-xfs-4/swdata -mserver=10.2.17.25:9333 -port=8001 -ip=10.2.1.4 -dataCenter=H11VS1
```