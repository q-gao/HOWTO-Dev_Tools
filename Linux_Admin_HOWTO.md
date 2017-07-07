# Linux Admin HOWTO

```sh
# Upgrade Ubuntu
sudo apt-get update
sudo apt-get upgrade
```

## mount

[udisks is modern replacement for mount](https://help.ubuntu.com/community/AutomaticallyMountPartitions)
> per user mount

`/etc/fstab`

```sh
sudo mount -t cifs //qgao3.example.com/C \
                   /local/mnt/workspace/qgao/qgao3_C/ \
                   -o credentials=/usr2/qgao/MyWork/credentials,rw,uid=qgao
```

where the `credentials` file looks like

```
Username=qgao
Password=******
Domain=NA
```
