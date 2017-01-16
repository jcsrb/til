# Use Github over alternative port

Pushing to Github when port 22 is blocked (often Airport WiFi) can be done over the alternative ssh port 443
to configure your system add the following to your ssh config (`~/.ssh/config`)
```
Host github.com
  Hostname ssh.github.com
  Port 443
```

more https://help.github.com/articles/using-ssh-over-the-https-port/
