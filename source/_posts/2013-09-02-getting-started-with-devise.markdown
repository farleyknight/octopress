---
layout: post
title: "Getting Started with Devise"
date: 2013-09-02 12:17
comments: true
categories:
---


## Before you dive in

Devise is a robust authentication framework for Rails. It covers 95% of the use authentication cases you'll ever have working with Rails. While it's very robust, that robustness comes at the cost complexity. It is not recommended for absolute beginners. Moderate beginners, those of you who have used Rails for small projects and understand how authentication works, should try some sandbox projects with Devise.

However, if you're learning Ruby on Rails at the same time as programming in general, I would personally recommend that you wait until your 2nd or 3rd year of programming before you dive into Devise.

With that preface aside, if you feel confident enough for what's ahead, go ahead and start reading. If you find yourself confused or asking yourself questions about terms & operations that I don't bother to explain, you should back up a bit and read my [Basic Rails Guide](/basic-rails-guide).


## Devise setup fast-track

If you're looking to get set up immediately, without delay, run this:

```bash
rails new my_website -m https://raw.github.com/farleyknight/rails_templates/master/devise.rb
```

Finer grained detail is explained below.

## Devise setup, step-by-Step

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

Come up with a descriptive name for your the "user" object in your project. Something simple like `Account`, `Admin`, `Client`, or even just `User` is good enough for our purposes. We'll assume that you're using User for the rest of this tutorial. Generate the `User` class by running:

```bash
rails g devise User
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

Each of those options to the `devise` method references a different devise module. That is covered in more detail in the [Basic Devise Guide](/basic-devise-guide/)

It also adds a database migration, which should look like this:

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


After doing a migrate:

```bash
rake db:migrate
```

You should now have a `users` table in your database now. After that, you're pretty much good to go! Run

```bash
rails server
```

and you should be able to login at `http://localhost:3000/users/sign_up`.

## Further Setup

If you need more control over how Devise works in your app, you're going to want to use their view generator as well. Run:

```bash
rails g devise:views
```

This adds a bunch of views under `app/views/devise/`. We can go into more detail under the [Basic Devise Customization](/basic-devise-customization/) guide.

