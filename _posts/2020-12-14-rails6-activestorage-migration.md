---
layout: default
title: Undefined method `name' for nil:NilClass in AddServiceNameToActiveStorageBlobs
published: true
tags: rails debugging
---

This blog posts explains how to get rid of the Undefined method error when
migration ActiveStorage from Rails 6.0 to 6.1.

<!--more-->

# Undefined method `name' for nil:NilClass in AddServiceNameToActiveStorageBlobs

You may have recently upgraded to Rails 6.1 and generated the
[ActiveStorage migrations](https://github.com/rails/rails/blob/5cfd58bbfb8425ab1931c618d98b649bab059ce6/activestorage/db/update_migrate/20190112182829_add_service_name_to_active_storage_blobs.rb)
to migrate to the new ActiveStorage version where multiple storage providers
can be used simultaneously.

If you get the following error:

```
NoMethodError: undefined method `name' for nil:NilClass
app/db/migrate/20201214195719_add_service_name_to_active_storage_blobs.active_storage.rb:7:in `up'
```

Make sure you have an ActiveStorage service defined in your environments
configuration file. This can either be `environments/development.rb`,
`environments/test.rb` or `environments/production.rb`. Make also sure it is
not commented out:

![screenshot of config lines in production.rb](/assets/active-storage.png)
