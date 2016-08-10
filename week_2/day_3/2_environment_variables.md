# Day 3

## Environment Variables

- We're now going to learn about environment variables. ENV (Environment) Variables are variables that differ depending on the environment you are at.

- There are generally 3 known environments - though it may differ on a case by case basis. They are:
  - development
  - test
  - production

- `Development` is the environment that you would work in.

- `Test` is the environment you run automated tests in.

- `Production` is the environment in live (the actual server your users would use)

- We'll now install [figaro](https://github.com/laserlemon/figaro) gem. (If you have done the heroku lesson, you can skip this)

- Type `figaro install` (Again, skip if you've done heroku lesson)

- This will append your `.gitignore` file and add the `config/application.yml` file.

- Inside your `config/application.yml` add the following:

  ```
  development:
    SERVER_URL: "http://localhost:3000"

  ```

- We've added a `SERVER_URL` variable now - let's go ahead and open your `password_reset_mailer.rb` and change the following:

  ```
    @url = "http://localhost:3000/password_resets/#{@user.password_reset_token}/edit"
  ```

- TO:

  ```
    @url = "#{ENV.fetch('SERVER_URL')}/password_resets/#{@user.password_reset_token}/edit"
  ```

- If you have pushed an `app` to `heroku`. You can configure your `production` url:

  ```
    production:
      SERVER_URL: "<your-heroku-url-here"
  ```
