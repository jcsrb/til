# Retry and remove now-working Dead Jobd from Sidekiq

run in inline mode in case your jobs spawn more jobs that you might not want to populate other queues

```ruby
require 'sidekiq/testing'
Sidekiq::Testing.inline!
query = Sidekiq::DeadSet.new      
query.each do |j|
  j.klass.constantize.new.perform(*j.args)
  j.delete 
rescue
 next
end
```

this will run and then remove jobs that work from the dead section of sidekiq
but in case they fail, keep the record
