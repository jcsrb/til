# Setup fzf in pry

* have fzf installed https://github.com/junegunn/fzf
* install the rb-readline gem `gem install rb-readline`

add this to you `~/.pryrc`

```ruby
require 'rb-readline'
require 'readline'
if defined?(RbReadline)
  def RbReadline.rl_reverse_search_history(sign, key)
    rl_insert_text  `cat ~/.pry_history | fzf --tac |  tr '\n' ' '`
  end
end
```

if you are in a rails context add to the gemfile
```ruby
group :developement
  gem 'rb-readline'
end
```

or more stuff in your `.pryrc
```ruby
if defined?(::Bundler)
  current_gemset = ENV['GEM_HOME']
  $LOAD_PATH.concat(Dir.glob("#{current_gemset}/gems/*/lib")) if current_gemset
end
```

Source:
- https://medium.com/@fangxing/how-to-use-fzf-in-irb-or-pry-for-search-command-history-6e87c9886e7
