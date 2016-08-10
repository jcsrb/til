# How to insert current timestamp into Cassandra

use `toTimestamp(now())`, 
where `now()` creates a *timeuuid* which you can convert to a timestamp

### Examnple

```SQL
CREATE TABLE IF NOT EXISTS users (
  userId uuid PRIMARY KEY,
  email text,
  created_at timestamp);
```
``` SQL
INSERT INTO users(userId, email, type, created_at) VALUES(
  uuid(),'jakob@somemail.com',toTimestamp(now()));
```

more https://docs.datastax.com/en/cql/3.3/cql/cql_reference/timeuuid_functions_r.html
