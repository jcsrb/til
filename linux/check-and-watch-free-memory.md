#check and watch free memory
to check system memory usage use the command `free` and use `-h` for better human readable numbers

example
```
[root@someserver ~]# free -h
             total       used       free     shared    buffers     cached
Mem:          1.8G       675M       1.2G       160K        58M       488M
-/+ buffers/cache:       127M       1.7G
Swap:           0B         0B         0B
```

to turn this into a watcher use `watch` with `-n 1` check every second
```
[root@someserver ~]# watch -n 1 free -h
Every 1.0s: free -h                                                 Fri Jan 20 10:39:59 2017

             total       used       free     shared    buffers     cached
Mem:          1.8G       676M       1.2G       160K        59M       489M
-/+ buffers/cache:       127M       1.7G
Swap:           0B         0B         0B

```

alternative use something like `top` or`htop` 
