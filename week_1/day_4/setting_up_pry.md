# Day 4 Lessons

## Setting up Pry

- Go to your `gemfile` and add `gem 'pry'` into your `development and test group`. Remove `byebug` gem and `bundle install`

- Example:

  ```
  group :development, :test do
    gem 'pry'
  end
  ```

- [Pry](https://github.com/pry/pry) is a runtime developer console that allows you to add breakpoints in your app by calling `binding.pry`

- This allows you to `pry` into your application to see what data or method is being called or passed around. It is extremely handy for debugging and learning.
