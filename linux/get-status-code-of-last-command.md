# Get the status code of the last command


```bash
echo $?
```

Example:
```bash
$ wget -q google.com 
$ echo $?
0
$ wget -q google.not-a-valid-tld 
$ echo $?
4
```
