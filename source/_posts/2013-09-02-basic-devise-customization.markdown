---
layout: post
title: "Basic Devise Customization"
date: 2013-09-02 14:15
comments: true
categories:
---


Sometimes setting up Devise is just the start of your app, and you need more customization to provide a robust user experience. Here's some common Devise customizations that should take you even further along than the [basic Devise guide](/getting-started-with-devise/) I've written before.

## Adding a Profile to your User model

Let's say you're building a social networking site. You want to keep a clean separation between the users _account_ (their email, password, account status, etc) from the user's _profile data_ (first & last name, avatar, location, relationship status, etc). The best way to handle this is to add another model for Profile, creating it alongside the User model during registration.

Here's how to customize the user model for exactly this use case.

```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :token_authenticatable, :confirmable,
  # :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable, :recoverable, :rememberable, :trackable, :validatable

  has_one :profile
  accepts_nested_attributes_for :profile
end
```

```ruby
class Profile < ActiveRecord::Base
  belongs_to :user
  validates :first_name, :last_name, :location, presence: true
end
```

Here's [an example app](http://github.com/farleyknight/devise_profiles/) that shows how all of this fits together:

## Signing up with Facebook, Twitter, etc

An often requested feature is to allow users to sign up via Facebook, Twitter, or other social network.

You'll need to add a few more gems to do this. You'll need to add to your Gemfile:

```ruby
gem 'devise' # Should already be in your Gemfile
gem 'omniauth'
gem 'omniauth-facebook'
```

### Devise Facebook fast-track

```ruby
rails new my_website -m https://raw.github.com/farleyknight/rails_templates/master/devise_facebook.rb
```


[discuss facebook integration]

Here's [an example app](http://github.com/farleyknight/devise_facebook/) that shows how all of this fits together.

