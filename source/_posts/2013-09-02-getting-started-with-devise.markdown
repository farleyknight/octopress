---
layout: post
title: "Getting Started with Devise"
date: 2013-09-02 12:17
comments: true
categories:
---


## Before you dive in

Devise is a robust authentication framework for Rails. It covers 95% of the use cases you'll ever have working with Rails. While it's very robust, it's also fairly complex and is not recommended for absolute beginners. If you're learning Ruby on Rails at the same time as programming in general, I would personally recommend that you wait until your 2nd or 3rd year of programming before you dive into Devise.

With that preface aside, if you feel confident enough for what's ahead, let's get started.

## Setting up

The following will get you the very basics for working with Devise. As with most Rails gems, you'll want to add the following line to your `Gemfile`.

```ruby
gem "devise"
```

And then run:

```bash
bundle install
```

In your Rails root directory. This will install Devise, and all it's dependencies. Once installed, run:

```ruby
rails g devise:install
```

This adds `config/initializers/devise.rb` to your Rails project. I provide complete details on what this file does in the [Basic Devise Customization](/basic-devise-customization/).

Come up with a descriptive name for your the "user" object in your project. Something simple like Account, Admin, Client, or even just User is good enough for our purposes. We'll assume that you're using User for the rest of this tutorial. Generate the User class by running:

```bash
rails g devise User
```

This adds a database migration to your project. Now run:

```bash
rake db:migrate
```

So your database now has a `users` table. After that, you're pretty much good to go! Run

```bash
rails server
```

and you should be able to login at `http://localhost:3000/users/sign_up`.

## Intermediate setup

If you need more control over how Devise works in your app, you're going to want to use their view generator as well. Run:

```bash
rails g devise:views
```

This adds a bunch of views under `app/views/devise/`. We can go into more detail under the [Basic Devise Customization](/basic-devise-customization/) guide.

## Would You Like to Know More?

* [Basic Devise Customization](/basic-devise-customization/)
* [Intermediate Devise Customization](/intermediate-devise-customization/)
* [Advanced Devise Customization](/advanced-devise-customization/)
* [Facebook Devise Integration](/facebook-devise-integration/)

