# Groupm by HOST part of URL

To group by the hostname of URLs and count them in PostgreSQL, you can use the REGEXP_REPLACE function along with GROUP BY and COUNT. Here’s a query to achieve that:

PG
```
SELECT 
    REGEXP_REPLACE(url, '^https?://([^/]+).*$', '\1') AS hostname,
    COUNT(*) AS count
FROM 
    your_table_name
GROUP BY 
    hostname;
```

slow as it likely won't hit indexes
