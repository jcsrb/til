# Check which application handles a specific mimetype 

this checks for the default http handler, AKA your browser
```
$ xdg-mime query default x-scheme-handler/http
firefox.desktop
```
Magnet links
```
xdg-mime query  default x-scheme-handler/magnet
ktorrent.desktop
```


More info and node package
https://github.com/kevva/xdg-default-browser
