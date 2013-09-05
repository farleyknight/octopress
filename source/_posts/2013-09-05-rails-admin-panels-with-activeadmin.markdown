---
layout: post
title: "Rails admin panels with activeadmin"
date: 2013-09-05 15:22
comments: true
categories:
---

couldn't find file 'jquery-ui'
  (in /home/umbrosus/.rvm/gems/ruby-1.9.3-p392@gancxadebebi/gems/activeadmin-0.5.1/app/assets/javascripts/active_admin/base.js:2)



The "jquery-rails" gem recently removed jQuery UI.

https://github.com/rails/jquery-rails/commit/2fdcdb2633cbc6426d412c050200fc31d14b9a3b

They recommend using the jquery-ui-rails gem.

There is an active pull request (as of this writing) to add that gem as a dependency. However, the developers of ActiveAdmin have stated that they are "locking it down until we officially drop support for Rails 3.0". The version they are locked to is jquery-rails < 3.0.0.

In the meantime, just modify your Gemfile:

gem "jquery-ui-rails" Not recommended, see @Kevin's comment below

Or you can downgrade your version of jquery-rails:

gem "jquery-rails", "< 3.0.0"
Or you can pull from their Github master branch. They have applied a temporary fix.

gem "activeadmin", github: "gregbell/active_admin"
