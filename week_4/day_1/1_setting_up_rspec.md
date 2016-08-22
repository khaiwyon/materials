# Day 1

## Setting up RSpec

- We've built our application, time to do some testing. Testing is an important part of software development. Softwares and Applications with good unit test coverage
tend to break or run into errors less, have better designs, and are more pleasant to maintain and develop.

- TDD is a software development method in which the developer creates a breaking test first, build the feature and get the test to work.

- We've not practiced this during your first project as we wanted you to get the hang of the Rails framework first.

- Now that we've finished it, it's time to write some tests for it.

- While rails has their own little framework for testing - [minitest](http://guides.rubyonrails.org/testing.html). We'll use another one that is widely popular - [RSpec](https://github.com/rspec/rspec-rails)

- Go ahead and install it:
  ```
  group :development, :test do
    gem 'rspec-rails', '~> 3.5'
  end
  ```

- Once you've installed it, run `rails generate rspec:install` in your terminal to generate a few files (refer to the documentation):
  - `.rspec`
  - `spec/rails_helper.rb`
  - `spec/spec_helper.rb`

- Next, we'll need to add a few more gems, I'll explain each one in detail eventually as we progress through the week, so here's a summary of it to start:

- DO NOT COPY PASTE, FIND THE APPROPRIATE GROUPS AND THE GEMS ACCORDINGLY.

  ```
  group :development, :test do
    gem 'factory_girl_rails'
    gem 'faker'
  end

  group :test do
    gem 'database_cleaner'
    gem 'shoulda-matchers'
    gem 'rails-controller-testing'
    gem 'capybara'
    gem 'selenium-webdriver'
  end
  ```

- Let's start with the easiest, [rails-controller-testing](https://github.com/rails/rails-controller-testing) is new to rails 5 since the team behind rails decided to drop controller tests altogether. We are still going to learn it however.

- [database_cleaner](https://github.com/DatabaseCleaner/database_cleaner) wipes the test database after every test suite to keep it clean.

- [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers) is a gem that eases `model` tests by providing a set of useful method calls for rspec.

- [factory_girl](https://github.com/thoughtbot/factory_girl) is a great gem for constructing database records for tests.

- [faker](https://github.com/stympy/faker) is a simple ruby gem for generating fake data.

- [capybara](https://github.com/jnicklas/capybara) and [selenium-webdriver](https://github.com/seleniumhq/selenium) are powerful gems for writing feature tests.

- Once you've done installing all your gems, we'll make some configuration changes, go to your `spec/rails_helper.rb` file and add the following code inside `Rspec.configure` block:

  ```
  RSpec.configure do |config|

    config.include FactoryGirl::Syntax::Methods

    config.before(:suite) do
      DatabaseCleaner.strategy = :transaction
    end

    config.after(:all) do
      DatabaseCleaner.clean_with(:truncation)
      FileUtils.rm_rf("#{::Rails.root}/public/uploads/test")
    end
  end
  ```

- `config.include FactoryGirl::Syntax::Methods` is straightforward, we are enabling `factory_girl` methods available to the tests.

  ```
  config.before(:suite) do
    DatabaseCleaner.strategy = :transaction
  end

  config.after(:all) do
    DatabaseCleaner.clean_with(:truncation)
    FileUtils.rm_rf("#{::Rails.root}/public/uploads/test")
  end
  ```

- The code above sets up `database_cleaner`. The first sets up DatabaseCleaner's strategy to `:transaction`. This is the fastest option as according to the documentation:
  `For the SQL libraries the fastest option will be to use :transaction as transactions are simply rolled back`.

- The code inside `config.after(:all)` block essentially cleans the database after each suite to keep it clean and fresh. Similarly `FileUtils.rm_rf` removes any file (in the case of this project - images) uploaded to the `public/uploads/test` folder - as determined by your `ImageUploader` file (`"uploads/test/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"`)

- Finally, let's run `rails:db:test` to prepare our test database.

- Now you're all set up to write your first test.
