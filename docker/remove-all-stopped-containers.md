# Remove All Stopped Continers

after playing arround locally you can end up with many containers
to clean up run

```bash
sudo docker rm $(sudo docker ps -a -q)
```
this actually tries to remove all containers, but will let you know that "_You cannot remove a running container_"

command might varrie based on OS, this is for linux
