---
layout: default
title: ActiveRecord::StrictLoadingViolationError for ActionText::RichText
published: true
tags: rails debugging
---

This blog posts explains how to get rid of the StrictLoadingViolationError when
using ActionText.

<!--more-->

# ActiveRecord::StrictLoadingViolationError for ActionText::RichText

You may have recently upgraded to Rails 6.1 and enabled the ActiveRecord
[strict loading setting](https://github.com/rails/rails/pull/39491)
(a setting which allows Rails to detect and throw errors when any
associations are loaded using N+1 queries).

This usually throws a bunch of errors on the first launch of the server
which are mostly easy to fix.

But there might be one particular instance which can be a tad harder to fix
than the rest. If you have a query similar to this:

```ruby
@posts = Post.where(category: 3).includes(:comments)
```

It will raise the following error:

```
`ActionText::RichText` called on `Post` is marked for strict_loading and cannot be lazily loaded.
```

Given the following model:

```ruby
class Post < ActiveRecord::Base
  has_many :comments
  has_rich_text :content
end
```

This error happens because the `ActionText` contents of the `Post` model are
stored in a seperate table. Since they are not included in the original
query and instead lazily loaded, Rails throws an error.

To include the `ActionText` in the original query as well, we have two
possibilities.

## Query for texts with attachments

If we use attachments in addition to `ActionText` itself, we can use the
`with_rich_text_<name>_and_embeds` method to load the texts and all associated
attachments. Make sure you replace `<name>` with the name of your attribute.

```ruby
@posts = Post.where(category: 3).with_rich_text_content_and_embeds.includes(:comments)
```

## Query for texts without attachments

If we don't use any attachments , we can use the `with_rich_text_<name>` method
to load the texts only.

```ruby
@posts = Post.where(category: 3).with_rich_text_content.includes(:comments)
```

As you can see, the `with_rich_text` methods can be combined with the usual
`includes` call to eager load any required data.
