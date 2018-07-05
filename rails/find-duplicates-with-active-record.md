# Use Active Record to find duplicate records


```ruby
User.select(:first,:email).group(:first,:email).having("count(*) > 1").size
```


```ruby
{[nil, nil]=>512,
 ["Joe", "test@test.com"]=>23,
 ["Jim", "email2@gmail.com"]=>36,
 ["John", "email3@gmail.com"]=>21}

```

[Source](https://stackoverflow.com/questions/21669202/find-rows-with-multiple-duplicate-fields-with-active-record-rails-postgres)
