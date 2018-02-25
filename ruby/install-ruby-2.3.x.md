# Installing Ruby 2.3.x on systems that use newer GCC

facing problems like
```
-- C level backtrace information -------------------------------------------
compiling loadpath.c
linking static-library libruby-static.a
ar: `u' modifier ignored since `D' is the default (see `U')
Segmentation fault (core dumped)
uncommon.mk:654: recipe for target 'enc.mk' failed
make: *** [enc.mk] Error 139
make: *** Waiting for unfinished jobs....
verifying static-library libruby-static.a
```

you have to get the older compiler and change the CC env path

```
sudo apt-get install gcc6 
CC=/usr/bin/gcc-6 rbenv install 2.3.3
```

More:
* https://bugs.ruby-lang.org/issues/14076
