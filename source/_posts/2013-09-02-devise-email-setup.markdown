---
layout: post
title: "Devise Email Setup"
date: 2013-09-02 14:43
comments: true
categories:
---


## Basic Setup

The most basic Devise email setup is to set configuration for `ActionMailer`, which is slightly different for `development` and `production` modes. However, for both modes you'll want to check out your `config/initializers/devise.rb` file and change the line that reads:

```ruby
config.mailer_sender = "please-change-me-at-config-initializers-devise@example.com"
```

To something a bit more sane, such as:

```ruby
config.mailer_sender = "support@mysite.com"
```

After setting this, all devise email will be sent as `support@mysite.com`.

## ActionMailer setup

All of this information is available in detail on the [official ActionMailer guide page](http://guides.rubyonrails.org/action_mailer_basics.html#action-mailer-configuration). However, this document will get you started faster, with some reasonable defaults.

### ActionMailer in `development` with Gmail

The best way to handle email in your `development` environment is with a throw-away Gmail account. I emphasize that it should be throw-away as we're including the email & password directly in the config file. If you're application has serious security implications, it's best that you use `sendmail` to do email during development, or that you pass the email & password via another configuration file that is not committed to your project (by ignoring it in `.gitignore`).

With that preface out of the way, in your `config/environments/development.rb` you'll want to have the following configuration:


```ruby
config.action_mailer.default_url_options = {:host => 'localhost:3000'}
config.action_mailer.delivery_method     = :smtp
config.action_mailer.smtp_settings       = {
   :tls            => true,
   :address        => "smtp.gmail.com",
   :port           => 587,
   :domain         => "gmail.com",
   :authentication => :login,
   :user_name      => "[username]",
   :password       => "[password]"
}
```

You'll want to replace `[email]` and `[password]` with your Gmail email & password.

You can see a working [example app here](http://github.com/farleyknight/devise_email).

### ActionMailer in `production`

Setting up ActionMailer in production requires a real domain name. So don't bother with this step until you've registered one. I suggest `namecheap.com`. Assuming you've just registered, if your domain name is `example.com`, go into your `config/environments/production.rb` and add the following lines:

```ruby
config.action_mailer.default_url_options = {:host => 'example.com'}
config.action_mailer.delivery_method     = :smtp
config.action_mailer.smtp_settings       = {
  :address => "127.0.0.1",
  :port    => 25,
  :domain  => 'example.com'
}
```

Afterwards, you'll want to set up a simple SMTP server on your web host. Run:

```gem install mailcatcher```

Will install the [mailcatcher](http://mailcatcher.me/) gem.


