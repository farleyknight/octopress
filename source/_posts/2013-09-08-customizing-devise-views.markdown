---
layout: post
title: "Customizing Devise views"
date: 2013-09-08 05:12
comments: true
categories:
---


## Customizing Devise Views

If you need more control over how Devise works in your app, you're going to want to use their view generator as well. Run:

```bash
$ rails g devise:views
```

This adds a bunch of views under `app/views/`. The output should look something like this:

```text
invoke  Devise::Generators::SharedViewsGenerator
create    app/views/devise/shared
create    app/views/devise/shared/_links.erb
invoke  form_for
create    app/views/devise/confirmations
create    app/views/devise/confirmations/new.html.erb
create    app/views/devise/passwords
create    app/views/devise/passwords/edit.html.erb
create    app/views/devise/passwords/new.html.erb
create    app/views/devise/registrations
create    app/views/devise/registrations/edit.html.erb
create    app/views/devise/registrations/new.html.erb
create    app/views/devise/sessions
create    app/views/devise/sessions/new.html.erb
create    app/views/devise/unlocks
create    app/views/devise/unlocks/new.html.erb
invoke  erb
create    app/views/devise/mailer
create    app/views/devise/mailer/confirmation_instructions.html.erb
create    app/views/devise/mailer/reset_password_instructions.html.erb
create    app/views/devise/mailer/unlock_instructions.html.erb
```

### Customizing Devise with Twitter Bootstrap



### Customizing user registration

