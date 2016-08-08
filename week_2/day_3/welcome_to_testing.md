# Day 3

## Welcome to Testing

- Running automated tests is an important part of software development. It cuts time wasted performing manual tests, allow better code design,
and covers blindspots if you ever change a feature that may indirectly affect an older feature in the near future.

- We're going to start by first testing the user's controller; but first, let's install all the necessary gems.

  ```
  group :development, :test do
    gem 'pry'
    gem 'factory_girl_rails'
    gem 'rspec-rails'
    gem 'faker'
  end

  group :test do
    gem 'database_cleaner'
    gem 'shoulda-matchers'
    gem 'rails-controller-testing'
    gem 'capybara'
  end
  ```

  #### The List of Gems
  - [RSpec Docs](https://www.relishapp.com/rspec/rspec-rails/docs), [RSpec Gem](https://github.com/rspec/rspec-rails)
  - [Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers)
  - [Factory Girls](https://github.com/thoughtbot/factory_girl)
  - [Database Cleaner](https://github.com/DatabaseCleaner/database_cleaner)
  - [Capybara](https://github.com/jnicklas/capybara)
  - [Rails Controller Testing](https://github.com/rails/rails-controller-testing)
  - [Faker](https://github.com/stympy/faker)
