# if using Vagnrant
with vagrant i was running into permssion denied error while mounting nfs on client.
below is the fix:
#### on nfs-server
 in etc/exports file
 ```
 /mnt/kubedata *(rw,sync,fsid=0,no_subtree_check,insecure)
 ```
  
 * sudo exportfs -rav
 * sudo systemctl stop nfs-kernel-server.service
 * sudo systemctl disable nfs-kernel-server.service
 * sudo systemctl enable nfs-kernel-server.service
 * sudo systemctl start nfs-kernel-server.service
 
### on client
sudo mount -t nfs4  192.168.33.10:/mnt/kubedata /nfs

# without vagrantIn this example we will setup a NFS Server and NFS client for persistent Data Storage

Server: 172.31.51.243
Client1 :172.31.58.117 
Client2 :172.31.56.226 

## 1. steps to perform on server 
```
sudo apt-get update
sudo apt install nfs-kernel-server

sudo mkdir /publicDir
sudo mkdir /privateDir

sudo chmod 777 /privateDir/
sudo chmod 755 /publicDir/
```

Then update the exports file
```
sudo nano /etc/exports

Editor:
/publicDir *(ro,sync,no_subtree_check)
/privateDir 172.31.58.117(rw,sync,no_subtree_check)
/privateDir 172.31.56.226(rw,sync,no_subtree_check)
```

check if everything is working fine

```
sudo exportfs -arvf

output:
exporting 172.31.58.117:/privateDir
exporting 172.31.56.226:/privateDir
exporting *:/publicDir
```

Start nfs server
```
sudo systemctl start nfs-kernel-server
sudo systemctl enable nfs-kernel-server



sudo systemctl status nfs-kernel-server

output
● nfs-server.service - NFS server and services
   Loaded: loaded (/lib/systemd/system/nfs-server.service; enabled; vendor preset: enabled)
   Active: active (exited) since Tue 2020-06-09 18:28:14 UTC; 8h ago
 Main PID: 3002 (code=exited, status=0/SUCCESS)
    Tasks: 0 (limit: 1152)
   CGroup: /system.slice/nfs-server.service

Jun 09 18:28:14 ip-172-31-51-243 systemd[1]: Starting NFS server and services...
Jun 09 18:28:14 ip-172-31-51-243 systemd[1]: Started NFS server and services.

```


## 2. Steps to perfom on client
```
sudo apt-get update
sudo apt-get install nfs-common

```
install nfs client package
```
showmount -e 172.31.51.243

output:
Export list for 172.31.51.243:
/publicDir  *
/privateDir 172.31.56.226,172.31.58.117
```

create directories for mount
```
sudo mkdir /mnt/public
sudo mkdir /mnt/private

sudo mount -t nfs 172.31.51.243:/publicDir /mnt/public
sudo mount -t nfs 172.31.51.243:/privateDir /mnt/private
```

check status 
```
mount
```

update /etc/fstab file for 

```
sudo nano /etc/fstab



172.31.51.243:/publicDir /mnt/public nfs defaults,_netdev 0 0
172.31.51.243:/privateDir /mnt/private nfs defaults,_netdev 0 0
```

run below commands to unmount temporary nfs with permanent nfs
```
sudo umount /mnt/public
sudo umount /mnt/private
mount

sudo mount -a
mount

```

Now NFS is setup you can place file in server to check, client can only read files in public but has access for both read/write in private
