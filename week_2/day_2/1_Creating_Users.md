# Day 2 Lessons

## Creating Users

- Now that we've built our topics, posts, and comments. We're going to finally add `users` into our app.

- Let's start by installing [bcrypt](https://github.com/codahale/bcrypt-ruby) gem.

- Next, we're going to create our `user` model. Referring to last week's chart. The attributes we need are:

  - email:string
  - password_digest:string
  - username:string
  - image:string
  - role:integer
  - password_reset_token:string
  - password_reset_at:datetime

- For this exercise, we are only going to use `email`, `password`, `image`, and `username` attributes first.

- Go ahead and create a new user model using `rails g model`

- Once your model is created, add [has_secure_password](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password) method to it:

  ```
  class User < ApplicationRecord
    has_secure_password
  end
  ```

- Now, we're going to create our `users controller`

- We are only going to need `new`, `edit`, `create`, and `update` actions.

- In your routes, add your `user` routes by calling `resources :users, only: [:new, :edit, :create, :update]`

- You can add the codes similarly as to how you would create a `comments` or `posts` controller. Remember to add your strong params.
