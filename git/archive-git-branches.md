# Archive Git branches

Problem: `git branch` too long ? lots of old stuff you not sure you need it but don't want to get rid off?

Solution: tag it and remove the branch

Archive
``` 
git tag archive/<branchname> <branchname>
git branch -d <branchname>
```

Restore 
```
git checkout -b <branchname> archive/<branchname>
```

If you want an aliases
```
git config --global alias.arc "! f() { git tag archive/$1 $1 && git branch -D $1;}; f"
git config --global alias.arcls "! f() { git tag | grep '^archive/';}; f"
```

or you can can hide merged branches 
```
git branch --no-merge master
```


Source && More:
* https://stackoverflow.com/questions/1307114/how-can-i-archive-git-branches
