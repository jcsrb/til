# Get CSV data into ruby by pasting to irb/console

```ruby
require 'csv'

# Prompt the user to enter CSV data
puts "Please paste the CSV data (end with an empty line):"

# Read the CSV data from the console
input_data = ""
while line = gets
  break if line.chomp.empty?
  input_data += line
end

# Parse the CSV data
csv_data = CSV.parse(input_data, headers: true)

# Print the parsed CSV data
csv_data.each do |row|
  puts row.to_hash
end
```
