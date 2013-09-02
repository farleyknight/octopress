---
layout: post
title: "Common Devise Problems"
date: 2013-09-02 12:35
comments: true
categories:
---

## No route matches [GET] "/users/sign_out"

Say you've got a logout partial that looks like this.


```erb
<div id="logout_link">
  <% if signed_in? %>
    <%= link_to "Logout", destroy_user_session_path %>
  <% end %>
</div>
```

You want to show a logout link while the user is signed in. However, when you pop the partial into the page, you get an error message:

```bash
No route matches [GET] "/users/sign_out"
```

The problem is your logout link. By default, Devise sessions use `[DELETE] "/users/sign_out"` and not `[GET] "/users/sign_out"`. In general, HTTP `DELETE` is preferred over `GET`, since it demonstrates that you're trying to remove a resource (namely the user session).

To fix this, you need to provide the `method` parameter to `link_to`. The fix looks like this:

```erb
<div id="logout_link">
  <% if signed_in? %>
    <%= link_to "Logout", destroy_user_session_path, :method => :delete %>
  <% end %>
</div>
```

You can read more about `link_to` [here](/rails-link_to/).

Alternatively, although I would not recommend it, you can change the HTTP method used for signing out in `config/initializers/devise.rb`. That would mean changing this line:

```ruby
config.sign_out_via = :delete
```

to this

```ruby
config.sign_out_via = :get
```

I would only use this in situations where you have several logout links all over your app, or you can't change the logout link directly. It's really a bit of a hack that Devise was polite enough to provide for you through there configuration, and not something that's recommended on standard Rails apps.
