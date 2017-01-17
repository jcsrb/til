# Use current timestamp

getting the timestamp
```
date -d "today" +"%Y%m%d%H%M"
```
then using it 

```
mysqldump -u root -p mydb | gzip > mydb_$(date -d "today" +"%Y%m%d%H%M").sql.gzip
```
generates `mydb_201701172218.sql.gzip`
