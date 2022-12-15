# Move an enqueue job from one queue to the other 

you can iterate over the jobs enqueued in a sidekiq queue
create new jobs with the exact parameters that you can configure using `set`

```ruby
query = Sidekiq::Queue['first_queue']
query.each do |job|
  # filter the jobs you want
  next unless job.tags.include? "cool_tag"
  next unless job.klass == "MyWorkerClass"
        
  job.klass.constantize.set(queue: 'other_queue', tags: job.tags + ["moved"]).perform_async(*job.args)
  
  job.delete 
end

```

