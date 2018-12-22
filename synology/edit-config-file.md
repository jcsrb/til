# Edit internal Config file for fan and temperature control

* enable ssh
* log in via ssh

```sh
# become root
sudo su

# make backups
cp /usr/syno/etc.defaults/scemd.xml /usr/syno/etc.defaults/scemd.xml.bak
cp /usr/syno/etc/scemd.xml /usr/syno/etc/scemd.xml.bak

# edit to your hearts desire
vi /usr/syno/etc.defaults/scemd.xml 
vi /usr/syno/etc/scemd.xml 

# kill scemd to reload
killall -9 scemd
```
