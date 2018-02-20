# Deal with UNASSIGNED shards


```
curl -XGET localhost:9200/_cat/shards?h=index,shard,prirep,state,unassigned.reason| grep UNASSIGNED
```

remove  `.watcher` and `.monitoring` 


```
curl -XDELETE 'localhost:9200/.watcher-history-*?pretty'
curl -XDELETE 'localhost:9200/.monitoring-*?pretty'
```

More:
* https://www.datadoghq.com/blog/elasticsearch-unassigned-shards/
