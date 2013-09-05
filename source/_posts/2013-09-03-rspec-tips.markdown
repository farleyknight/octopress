---
layout: post
title: "RSpec Tips"
date: 2013-09-03 13:07
comments: true
categories:
---

## Rspec generators

Use this command to generate a spec for an existing ModelName

```bash
rails g spec:model ModelName
```

## Test files under `lib`

Create `spec/lib`

```bash
$ mkdir -p spec/lib
```

Add the following line to `spec/spec_helper.rb`

```ruby
RSpec.configure do |config|
  config.pattern = "**/*_spec.rb"
  # A bunch more code..
end
```

