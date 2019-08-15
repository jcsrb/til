## Run a migration from the rails console

! do not do this for production ! 

```ruby
ActiveRecord::Migration.remove_column :table_name, :column_name
ActiveRecord::Migration.add_column :table_name, :column_name, :string
```
