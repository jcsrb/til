# A few ways to get extra space in fedora

_think if you really need those docker images_
```sh
sudo journalctl --vacuum-size=100M 
pkcon refresh force
sudo dnf clean all
sudo docker image prune
sudo docker container prune
sudo docker volume prune
```
