# Fix Keybase Git not working on macos

after some updates the keybase git was no longer working because of a path issue
getting the error `git: 'remote-keybase' is not a git command.`
I needed to add `/Applications/Keybase.app/Contents/SharedSupport/bin/` to my path to make it work again
 
```
❯ git pull
git: 'remote-keybase' is not a git command. See 'git --help'.
❯ export PATH="/Applications/Keybase.app/Contents/SharedSupport/bin/:$PATH"
❯ git pull
Initializing Keybase... done.
Syncing with Keybase... done.
Counting and decrypting: 13 objects... done.
```
