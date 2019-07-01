# Group records by a range of a timpestamp , week, month, day 


```ruby
pp Article.group("DATE_TRUNC('week', created_at)").count
```

```ruby
pp Article.group("DATE_TRUNC('month', created_at)").count
#=> {"2019-05-01T00:00:00.000Z"=>59661,
# "2019-06-01T00:00:00.000Z"=>421135,
# "2019-07-01T00:00:00.000Z"=>31456}
```
