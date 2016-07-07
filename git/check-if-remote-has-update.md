# Check which remote branches have been updated without pulling or fetching

```
[me@myserver app-folder]$ git remote -v update
Fetching origin
remote: Counting objects: 11, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 11 (delta 9), reused 11 (delta 9), pack-reused 0
Unpacking objects: 100% (11/11), done.
From github.com:me/repo
   ee5029b..0a72a10  master     -> origin/master
```
