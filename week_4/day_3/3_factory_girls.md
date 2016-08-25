# Day 2

## Factory Girls

- Factory girls are a great way to quickly create new records for your tests.

- Let's create our first factory:

- Create a new folder inside `spec` called `factories`

- In your newly created folder, add a file called `users.rb`

- The basic code:

```
  FactoryGirl.define do
    factory :user do
    end
  end
```

- In all our factories, this is the basic template for all your factories. Note that the name of the factory should match the name of the model without the capital letter. In this case, it's `:user` for our `User` model.

- Now we'll add the basic attributes:

```
  email "user@email.com"
  username "bob"
  password "password"
```

- Now, by default, when we create a new user from the factory - it will default to the data above. Sometimes however, there may be certain changes we'd like to make when generating new records. For example, we may need to create two different users. The method above won't work now simply because it's static. This is where `traits` come in.

- Check the code below, I'll explain bit by bit:

```
  trait :with_image do
    image { fixture_file_upload("#{::Rails.root}/spec/fixtures/cat.jpg") }
  end

  trait :sequenced_email do
    sequence(:email) { |n| "user#{n}@email.com" }
  end

  trait :sequenced_username do
    sequence(:username) { |n| "username#{n}@email.com" }
  end

  trait :admin do
    role :admin
    sequence(:email) { |n| "admin#{n}@email.com" }
    sequence(:username) { |n| "admin#{n}" }
  end

  trait :moderator do
    role :moderator
    sequence(:email) { |n| "moderator#{n}@email.com" }
    sequence(:username) { |n| "moderator#{n}" }
  end
```

### Traits

- `traits` are a way to override certain default assignments or add additional ones when creating new records.

- For example, the `trait :with_image` adds an image when creating a new user as well.

- `fixture_file_upload` is a method for used for uploading files (in this case our user's profile image) for testing. `"#{::Rails.root}/spec/fixtures.cat.jpg"` is the `url` of the image we've stored it for usage. In this case, I have `cat.jpg` saved inside the folder `spec/fixtures`. `"#{::Rails.root}"` is the method to fetch the root folder of your rails application.

- Additionally, to get `fixture_file_upload` to work, we'll need to include it at the top of your factory. (`include ActionDispatch::TestProcess`)

- Create a new folder called `fixtures` inside `spec` folder and add a nice image of your choice.

- `sequence(:email)` is great when you want to create multiple users. How it works is that you call the method `sequence` and pass in the name of the attribute (in this case it's the user's email) as an argument and a proc (`{}`).

- From the example, `sequence(:email) { |n| "user#{n}@email.com"}`:
  - For `x` number of times, create a new user with the email "user1@email.com", "user2@email.com", etc etc

- Now, let's do a little refactoring, on your controller tests you've build yesterday. Instead of `@user = User.create()`, you can use `create(:user)` instead.

- A FEW KEY THINGS TO NOTE:

- You can override your data like this: `create(:user, email: "override@email.com", username: "override@email.com")`

- Traits are passed in the same way:- `create(:user, :admin, :with_image, <:name-of-trait>)`. In this case, we've called the trait :admin and :with_image.

- You can also use both: `create(:user, :admin, email: "override@email.com")`

### Challenge

- Build factories for `topics`, `comments`, and `posts`.

- Replace all your `Model.create` in your controller tests with factory girl methods.
