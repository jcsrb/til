# Intent to Add 

if you like `git add -p` but are annoyed that you cant go throw untracked files this will help:
```Bash
git add --intent-to-add my-untracked-file.md
# or short
git add -N my-untracked-file.md
```
Intent to Add tells git to index the file but not add the content yet.

From the man page:

> Record only the fact that the path will be added later. An entry for the
> path is placed in the index with no content. This is useful for, among other
> things, showing the unstaged content of such files
> with git diff and committing them with git commit -a.

* Source: https://robots.thoughtbot.com/intent-to-add
* Source: https://github.com/jbranchaud/til/blob/master/git/intent-to-add.md
