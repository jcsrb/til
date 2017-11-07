# what is using that port

```
lsof -wni tcp:3000
```

Example

```
$ lsof -wni tcp:3000
COMMAND    PID  USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
ruby      9537 jakob    9u  IPv4 1223963      0t0  TCP *:3000 (LISTEN)
ruby      9550 jakob    9u  IPv4 1223963      0t0  TCP *:3000 (LISTEN)
ruby      9550 jakob   15u  IPv4 1223173      0t0  TCP 127.0.0.1:3000->127.0.0.1:52280 (CLOSE_WAIT)
chrome  129513 jakob  347u  IPv4 1220489      0t0  TCP 127.0.0.1:52436->127.0.0.1:3000 (ESTABLISHED)
```
