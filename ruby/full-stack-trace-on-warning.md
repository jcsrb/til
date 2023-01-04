# Get Full Stacktrace on ruby warnings

if you have warnings but they are unclear where they happen, extend (temporarly for development) the warning class to print the full `caller` stacktrace

```ruby
module Warning
  def warn(msg)
    puts caller
    puts msg
  end
end
```
