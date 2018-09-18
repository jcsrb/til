# Run any SQL you need from the rails console


```ruby
res = ActiveRecord::Base.connection.exec_query(sql_string)
```
