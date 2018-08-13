# Walkthrough of creating and deleting an environment variable in bash:

Test if the DUALCASE variable exists:
```
el@apollo:~$ env | grep DUALCASE
el@apollo:~$ 
```

It does not, so create the variable and export it:
```
el@apollo:~$ DUALCASE=1
el@apollo:~$ export DUALCASE
```

Check if it is there:
```
el@apollo:~$ env | grep DUALCASE
DUALCASE=1
```

It is there. So get rid of it:
```
el@apollo:~$ unset DUALCASE
```

Check if it's still there:
```
el@apollo:~$ env | grep DUALCASE
el@apollo:~$ 
```
The DUALCASE exported environment variable is deleted.

Extra commands to help clear your local and environment variables:
Unset all local variables back to default on login:

```
el@apollo:~$ CAN="chuck norris"
el@apollo:~$ set | grep CAN
CAN='chuck norris'
el@apollo:~$ env | grep CAN
el@apollo:~$
el@apollo:~$ exec bash
el@apollo:~$ set | grep CAN
el@apollo:~$ env | grep CAN
el@apollo:~$
```
`exec bash` command cleared all the local variables but not environment variables.

Unset all environment variables back to default on login:

```
el@apollo:~$ export DOGE="so wow"
el@apollo:~$ env | grep DOGE
DOGE=so wow
el@apollo:~$ env -i bash
el@apollo:~$ env | grep DOGE
el@apollo:~$
```
`env -i bash` command cleared all the environment variables to default on login.

source: https://stackoverflow.com/a/24026737/54988
