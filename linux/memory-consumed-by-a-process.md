# Get memory used of a process 

if % is good enough you use

```
ps -C node -o pid,%cpu,%mem,cmd
```

if you want to humannized version of the memory, you need to run this 
```
ps -C node -o size,pid,user,command --sort -size | awk '{ hr=$1/1024 ; printf("%13.2f Mb ",hr) } { for ( x=4 ; x<=NF ; x++ ) { printf("%s ",$x) } print "" }'
```
