# Import a mongo bson export 

basics:
```
mongorestore -d db_name -c collection_name path/file.bson
```

Incase only for a single collection. Try this:

```
mongorestore --drop -d db_name -c collection_name path/file.bson
```

For restoring the complete folder exported by mongodump:

```
mongorestore -d db_name path/
```


Source and more 
* https://stackoverflow.com/questions/6770498/how-to-import-bson-file-format-on-mongodb
