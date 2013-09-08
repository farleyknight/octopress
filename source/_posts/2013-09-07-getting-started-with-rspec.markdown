---
layout: post
title: "Getting started with RSpec"
date: 2013-09-07 14:41
comments: true
categories:
---

# What is RSpec?

[RSpec](http://rspec.info/) is a Ruby gem for writing more elegant tests. It's known for being oriented for Behavior Driven Development.

# Installation

## With a Gemfile

If your project has Gemfile, add these lines to it:

```ruby
group :test do
  gem 'rspec'
end
```

And then run

```bash
$ bundle
```

to install the gem.

## Without a Gemfile

Install the `rspec` gem the old-fashioned way:

```bash
$ gem install rspec
```

## Initialization

Next, you can initialize your project (can be a Rails app, Sinatra app, or even a Ruby gem) with the necessary `rspec` files:

```bash
$ rspec --init
```

This will add `spec/spec_helper.rb` and `.rspec` to your current directory.

