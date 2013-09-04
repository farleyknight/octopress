---
layout: post
title: "Awesome Rails Tools"
date: 2013-09-03 13:24
comments: true
categories:
---

## zeus

[zeus](https://github.com/burke/zeus) gives you lightning fast Rails boot times. Starts a master Rails process & spawns new processes for `rails console`, `rails server`, `rails generate`, etc.

### How to use

First off you'll want to install `zeus`.

```bash
gem install zeus
```

Notice that you **don't** need to add it to your `Gemfile`. It is ideally only run locally. Making it a project dependency can cause server maintenance problems down the line (see [Common Zeus Problems](#common_zeus_problems) below). After installing, you can boot up `zeus` with the command:

```bash
zeus start
```

Here's what it looks like on my Ubuntu laptop:

[screenshot here]

### Common Zeus Tasks

To run the Rails console:

```bash
zeus console
```

To run the Rails generator:

```bash
zeus generate model Book
```

To run the Rails sever:

```bash
zeus sever
```

### Common Zeus Problems

While `zeus` is an awesome gem, getting it working without any issues can be a bit of a problem at times. Here's some common problems that can occur with `zeus` and Rails.

#### zeus rspec

If you're testing with RSpec. If you try `zeus rspec`, you'll likely get:

```bash
`spec_file?': undefined method `match' for nil:NilClass (NoMethodError)
```

Instead run `rspec`:

```bash
zeus test spec
```

#### RSpec tests running twice

After getting your rspec tests running, you'll probably notice that it's running them twice. [The solution](https://github.com/burke/zeus/issues/180#issuecomment-12758345) is to remove the line

```ruby
require 'rspec/autorun'
```

from your `spec/spec_helper.rb`

#### Running zeus on ARM (Chromebook or Raspberry Pi)

While `zeus` is a great tool, it has it's limitations. It cannot be used on ARM chips, namely Chromebooks & Raspberry Pi. If you're looking for this functionality on those machines (or other chips that might not be supported by `zeus`) I would suggest checking out `spring`, which is discussed below.

#### By default, zeus can't handle assets

Many of the `zeus rake ...` commands work out of the box, including `zeus rake db:migrate`. However, `rake` commands involving the asset pipeline do not work as well. For example, trying `zeus rake assets:precompile` will get you:

```bash
$ zeus rake assets:clean

/home/farleyknight/.rvm/rubies/ruby-1.9.3-p448/bin/ruby zeus command: rake assets:clean:all RAILS_ENV=development RAILS_GROUPS=assets
/home/farleyknight/.rvm/rubies/ruby-1.9.3-p448/bin/ruby: No such file or directory -- zeus command: rake (LoadError)
rake aborted!

Command failed with status (1): [/home/farleyknight/.rvm/rubies/ruby-1.9.3-...]
```

The problem is that zeus doesn't change the `ENV['RAILS_GROUP']` for `rake assets:precompile` to work.

The fix is as follows.

First, make sure you have the line in your current environment file:

```ruby
config.assets.initialize_on_precompile = false
```

Next run:

```bash
zeus init
```

It will create `zeus.json` and `custom_plan.rb`. Change your custom plan to match:

```ruby
require 'zeus/rails'
require 'zeus/rails'

# Usage: `zeus assets some:nested:task'
#
# translates to
#
# `rake assets:some:nested:task`
#
# E.g.
#
# `zeus assets precompile`
#
# instead of `rake assets:precompile`

class AssetsPlan < Zeus::Rails
  def assets_environment
    Bundler.require(:development, :assets)
    ::Rails.env = ENV['RAILS_ENV'] = "development"
    ENV['RAILS_GROUPS'] = "assets"
  end

  def assets
    ARGV.first.prepend "assets:"
    Rake.application.run
  end
end

Zeus.plan = AssetsPlan.new
```


And change `zeus.json` to match:

```json
{
  "command": "ruby -rubygems -r./zeus_assets -eZeus.go",
  "plan": {
    "boot": {
      "default_bundle": {
        "assets_environment": {
          "assets": ["a"]
        },
        "development_environment": {
          "prerake": {
            "rake": []
          },
          "runner": ["r"],
          "console": ["c"],
          "server": ["s"],
          "generate": ["g"],
          "destroy": ["d"],
          "dbconsole": []
        },
        "test_environment": {
          "test_helper": {"test": ["rspec", "testrb"]}
        }
      }
    }
  }
}
```


This will add the command `zeus assets ...`, so for example `zeus assets clean` will run `rake assets:clean` and `zeus assets precompile` will run `rake assets:precompile` and so on.

If you're interested on how the `Zeus::Rails` plans work, [checkout the documentation](https://github.com/burke/zeus/blob/master/docs/ruby/modifying.md) on the [github repository](https://github.com/burke/zeus/).

## spring

Another framework for handling quick Rails boot times is a tool called [spring](https://github.com/jonleighton/spring). It has many features that `zeus` has, but it's written in pure Ruby.

### Getting Started

[include how to use spring]
