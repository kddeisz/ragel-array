# Ragel::Array

[![Build Status](https://github.com/kddnewton/ragel-array/workflows/Main/badge.svg)](https://github.com/kddnewton/ragel-array/actions)
[![Gem Version](https://img.shields.io/gem/v/ragel-array.svg)](https://rubygems.org/gems/ragel-array)

[Ragel](https://www.colm.net/open-source/ragel/) generates ruby code with very large arrays of integers that allocate a lot of memory when required. To reduce memory consumption, this gem replaces those arrays with strings that transform into native `uint32_t` arrays.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'ragel-array'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ragel-array

## Usage

After you've run `ragel` to generate your parser, you should then run this gem over the resulting source file and it will replace the integer arrays inline. For example, the following code adds a rake rule to generate the ragel parser from the grammar file and then run `Ragel::Array` over it:

```ruby
rule %r|_parser\.rb\z| => '%X.rl' do |t|
  sh "ragel -s -R -L -F1 -o #{t.name} #{t.source}"
  require 'ragel/array'
  Ragel::Array.replace(t.name)
end
```

Then, in your application, add `require 'ragel/array'` before you require your parser. Now you should be off and running!

We also provide a `ragel-array` command that you can execute once this gem is installed. Usage looks like `ragel-array [path]` where `path` is a path to a generated ragel parser.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/kddnewton/ragel-array.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
