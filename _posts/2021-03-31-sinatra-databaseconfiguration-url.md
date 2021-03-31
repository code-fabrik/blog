---
layout: default
title: 'NameError: uninitialized constant ActiveRecord::DatabaseConfigurations::ConnectionUrlResolver::URI'
published: true
tags: sinatra activerecord debugging
---

This blog posts explains how to get rid of the NameError when
using the sinatra-activerecord gem with ActiveRecord 6.1.

<!--more-->

# NameError: uninitialized constant ActiveRecord::DatabaseConfigurations::ConnectionUrlResolver::URI

You may have recently used [sinatra-activerecord](https://github.com/sinatra-activerecord/sinatra-activerecord)
while also using `ActiveRecord` 6.1+. Due to a recent update, `sinatra-activerecord` is no longer out of the box
compatible with `ActiveRecord` 6.1.

If you get the following error when running your migrations:

```
NameError: uninitialized constant ActiveRecord::DatabaseConfigurations::ConnectionUrlResolver::URI
/app/vendor/bundle/ruby/2.7.0/gems/activerecord-6.1.3.1/lib/active_record/database_configurations/connection_url_resolver.rb:47:in `uri_parser'
```

You can fix the error by requiring the `URI` class before loading `ActiveRecord`, for example in your `app.rb` file. This makes sure that the `URI` class can be found:

![](/assets/activerecord-require-uri.png)
