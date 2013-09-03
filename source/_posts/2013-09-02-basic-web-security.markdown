---
layout: post
title: "Basic Web Security"
date: 2013-09-02 14:35
comments: true
categories:
---

## How to handle passwords

You want to avoid passwords as much as possible when developing. This is not just a convenience measure, but also an engineering measure. Each human-memorized password will be another point of attack for your web server and web application. Whenever possible encrypted keys are preferable to passwords, as they tend to be much longer (and thus harder to brute-force), require no memorization, and can be locked away on thumbdrives or on the filesystem.

### Set up passwordless SSH

If you've got a web server which you can SSH into, you should immediately remove route around the need for passwords by running this command:

```ssh-copy-id [myaccount]@[mysite]```

If you run this command (replacing `[myaccount]` with your SSH login and `[mysite]` with your site's domain name), you'll be asked one more time to login with a password.


### Put passwords into config files, not in source code



### Put passwords into a password manager. Don't bother memorizing them.
