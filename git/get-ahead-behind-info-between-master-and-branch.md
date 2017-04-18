## Get ahead/behind info between master and branch


Get the values of Github like 
> This branch is 1 commit ahead and 2 commits behind master 
locally 

```
git rev-list --left-right --count master...my-branch
```
example output 

```
38      41
```

Source:
* http://stackoverflow.com/questions/20433867/git-ahead-behind-info-between-master-and-branch/27940027
