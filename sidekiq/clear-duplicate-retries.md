# Clear duplicated jobs retries from the Sidekiq retry Queue

```ruby
query = Sidekiq::RetrySet.new
counts = {}

query.each do |j|
  signature = [j.item["class"],j.item["args"],j.item["error_class"],j.item["error_message"]]
  counts[signature] ||= 0
  counts[signature] += 1
  j.delete if counts[signature] > 1
end
```
