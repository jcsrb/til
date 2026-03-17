# Per-directory git email

Problem: you use a personal email for git globally but need a work email for all repos under a specific folder (e.g. `~/work/acme/`).

Solution: use `includeIf` in your global `~/.gitconfig` to conditionally load a separate config.

Create `~/.gitconfig-acme`:
```
[user]
	name = Jane Doe
	email = jane@acme.com
```

Add this to your `~/.gitconfig`:
```
[includeIf "gitdir:~/work/acme/"]
	path = ~/.gitconfig-acme
```

The trailing `/` is important — it matches all subdirectories.

Verify:
```
cd ~/work/acme/some-repo
git config user.email
# jane@acme.com

cd ~/personal/some-repo
git config user.email
# jane@personal.email
```

Source && More:
* https://git-scm.com/docs/git-config#_conditional_includes
