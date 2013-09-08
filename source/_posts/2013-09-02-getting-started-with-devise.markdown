---
layout: post
title: "Getting Started with Devise"
date: 2013-09-02 12:17
comments: true
categories:
---

## Before you dive in

[Devise](https://github.com/plataformatec/devise) is a robust authentication framework for Rails. It covers 95% of the authentication use cases you'll ever have working with Rails. While it's very robust, that robustness comes at the cost of complexity. It is not recommended for absolute beginners. Moderate beginners, in other words, those of you who have used Rails for small projects and understand how authentication works in general, should try some sandbox projects with Devise.

However, if you're learning Ruby on Rails at the same time as programming in general, I would personally recommend that you wait until your 2nd or 3rd year of programming before you dive into Devise.

With that preface aside, if you feel confident enough for what's ahead, go ahead and start reading. If you find yourself confused or asking yourself questions about terms & operations that I don't bother to explain, you should back up a bit and read my [Basic Rails Guide](/basic-rails-guide/).

## Devise setup fast-track

If you're looking to get set up immediately, without delay, you can create a fresh, new Rails app with this template:

```bash
rails new my_website -m https://raw.github.com/farleyknight/rails_templates/master/devise.rb
```

If you're reading this tutorial slowly, to pick up on all the fine-grained details, you can use the template above to create an application to use as a reference as you read.

## Devise setup, step-by-Step

The following will get you the very basics for working with Devise. As with most Rails gems, you'll want to add the following line to the bottom of your `Gemfile`.

```ruby
gem "devise"
```

On the command line, run:

```bash
$ bundle install
```

In your Rails root directory. This will install Devise, and all it's dependencies. Once installed, run on the command line:

```bash
$ rails g devise:install
```

This adds `config/initializers/devise.rb` to your Rails project. I provide complete details on what this file does in the [Basic Devise Customization](/basic-devise-customization/).

Come up with a descriptive name for the "user" object in your project. Something simple like `Account`, `Admin`, `Client`, or even just `User` is good enough for this tutorial. We'll assume that you're using `User` for the rest of this tutorial. Generate the `User` class by running:

```bash
$ rails g devise User
```

It should look like this:

```ruby
class User << ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :token_authenticatable, :confirmable,
  # :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable, :recoverable, :rememberable, :trackable, :validatable
end
```

Each of those options to the `devise` method references a different devise module. That is covered in more detail in the [Basic Devise Guide](/basic-devise-guide/). It also adds a database migration, found under `db/migrate/..._devise_create_users.rb`, which should look like this:

```ruby
class DeviseCreateUsers < ActiveRecord::Migration
  def change
    create_table(:users) do |t|
      ## Database authenticatable
      t.string :email,              :null => false, :default => ""
      t.string :encrypted_password, :null => false, :default => ""

      ## Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at

      ## Rememberable
      t.datetime :remember_created_at

      ## Trackable
      t.integer  :sign_in_count, :default => 0
      t.datetime :current_sign_in_at
      t.datetime :last_sign_in_at
      t.string   :current_sign_in_ip
      t.string   :last_sign_in_ip

      ## Confirmable
      # t.string   :confirmation_token
      # t.datetime :confirmed_at
      # t.datetime :confirmation_sent_at
      # t.string   :unconfirmed_email # Only if using reconfirmable

      ## Lockable
      # t.integer  :failed_attempts, :default => 0 # Only if lock strategy is :failed_attempts
      # t.string   :unlock_token # Only if unlock strategy is :email or :both
      # t.datetime :locked_at

      ## Token authenticatable
      # t.string :authentication_token


      t.timestamps
    end

    add_index :users, :email,                :unique => true
    add_index :users, :reset_password_token, :unique => true
    # add_index :users, :confirmation_token,   :unique => true
    # add_index :users, :unlock_token,         :unique => true
    # add_index :users, :authentication_token, :unique => true
  end
end

```

After doing a migrate, run the `rake` command:

```bash
$ rake db:migrate
```

You should now have a `users` table in your database now. After that, you're pretty much good to go! Run

```bash
$ rails server
```

and you should be able to login at `http://localhost:3000/users/sign_up`.

## Handling Authentication

It's not simply enough to allow users to log in to your site. You need to give your logged in users an incentive to log in! Here's how to set up your site to react differently, based on whether they've logged in or not.

### Using login information in your views

Most sites typically have a spot in the upper-right hand corner for logging into the site. For example, we might have a header toolbar, with the words "Log into mysite.com". Similarly, after the user has logged in, that wording could change to say "Logged in as: farleyknight@gmail.com". Using the `user_logged_in?` and `current_user` methods, the code would look like this:

```erb
<% if user_signed_in? %>
  Logged in as: <%= current_user.email %>
<% else %>
  <%= link_to "Log into mysite.com", new_user_session_path %>
<% end %>
```

### Checking if the user is logged in, per controller action

Once you have basic authorization in place, you'll want to mark off sections of the site for logged-in users only. Let's say you have a `DashboardController` which is the dashboard for any logged in user. Using the before filter method `authenticate_user!`, we can be sure that this dashboard is only seen by logged in users.


```ruby
class DashboardController < ApplicationController
  before_filter :authenticate_user!

  def index
    # Show some stuff to the logged-in user
  end
end
```

### User Redirection

After your user has logged in, you want to redirect them to their dashboard. To do this, add the following method to your `ApplicationController`:

```ruby
class ApplicationController < ActionController::Base
  # ...
  def after_sign_in_path_for(user)
    dashboard_path
  end
end
```

After a user has logged out, you want to redirect them to a special page, thanking them and asking them to return soon. Here's another snippet you can use to do that:


```ruby
class ApplicationController < ActionController::Base
  # ...
  def after_sign_out_path_for(user)
    please_come_again_path
  end
end
```

## Further Setup

If you need more control over how Devise works in your app, you're going to want to use their view generator as well. Run:

```bash
$ rails g devise:views
```

This adds a bunch of views under `app/views/devise/`. We can go into more detail under the [Basic Devise Customization](/basic-devise-customization/) guide.

Some other things you may want to do with your devise installation. You may want allow users to:

* [Allow Users to edit their password](/devise-edit-passwords/)
* [Confirm their accounts via email](/devise-user-confirmation/)
* [Invite other users via email](/devise-user-invites/)
* [Advanced User authorization with CanCan and Rolify](/devise-cancan-rolify/)
* Allow users to sign up via
  * [Facebook](/devise-facebook/)
  * [Twitter](/devise-twitter/)
  * [Google+](/devise-google-plus/)


Check out my full list of [my Devise tutorials](/devise-tutorials/) if you are stil curious.



