# Quick Way to list DB Indexes in rails console


```ruby
  ActiveRecord::Base.connection.tables.each do |table|
  indexes = ActiveRecord::Base.connection.indexes(table)
  if indexes.length > 0
    puts "====>  #{table} <===="
    indexes.each do |ind|
      puts "----> #{ind.name}"
    end
    puts "====>  #{table} <===="
    2.times{ puts ''}
  end
end
```
