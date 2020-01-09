# Backup Page

```
wget -H -E -k -p SOMEURL
```


Those wget options:

* `-p`: Get any dependencies needed for offline viewing
* `-H`: Include dependency files from other hosts
* `-k`: Update paths in the local HTML to point at local files
* `-E`: Add .html to the file name if it is an HTML file but doesn't have a suitable extension
