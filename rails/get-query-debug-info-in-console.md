### Get More details on your AR query when running from the console

change the AR logger to the STDOUT
```
ActiveRecord::Base.logger = Logger.new(STDOUT)
```

now running queryies give you debug info, like the query and duration
```
irb(main):008:0> Item.order(created_at: :desc).limit(4)      
D, [2017-03-15T15:12:48.035201 #3691] DEBUG -- : {:message=>"Item Load", :sql=>"SELECT  \"items\".* FROM \"items\" ORDER BY \"items\".\"created_at\" DESC LIMIT $1", :duration=>185.764072, :binds=>{"LIMIT"=>4}}
```
