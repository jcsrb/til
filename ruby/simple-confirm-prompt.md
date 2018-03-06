# Simple confirmation prompt in ruby

```ruby
printf "WARNING - press 'y' to continue: "
prompt = STDIN.gets.chomp
return unless prompt == 'y'
```
