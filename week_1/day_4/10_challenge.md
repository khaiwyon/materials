# Day 4

## Challenge

- Add Styling to your `index, new, and edit` pages for `topics`.

- Add a new link on your header that sends you to `topics index` page.

- Using what you've learnt with today, build the same thing for `posts`.

NOTE: Temporarily for this challenge. Modify this on your `Post` model to add `optiona: true` to `belongs_to :topic`

```
  class Post < ApplicationRecord
    has_many :comments
    belongs_to :topic, optional: true
  end
```
