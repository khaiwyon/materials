## Uploading to HEROKU

- Warning: You will need a credit card for this. The service is free if you choose their free servers, however a card is required for verification.

- Install your heroku toolbelt [here](https://toolbelt.heroku.com/)

- Referring this [documentation](https://devcenter.heroku.com/articles/creating-apps)

- On the directory of your app, type `heroku create <your-app-name>`.

- You should see heroku creating a new application for you.

- You can check it on your heroku dashboard (www.heroku.com)

- We'll now make some configuration changes to your server. Let's open `config/puma.rb` and make the following changes:

  ```
  # Puma can serve each request in a thread from an internal thread pool.
  # The `threads` method setting takes two numbers a minimum and maximum.
  # Any libraries that use thread pools should be configured to match
  # the maximum value specified for Puma. Default is set to 5 threads for minimum
  # and maximum, this matches the default thread size of Active Record.
  #
  threads_count = ENV.fetch("RAILS_MAX_THREADS") { 2 }.to_i
  threads threads_count, threads_count

  # Specifies the `port` that Puma will listen on to receive requests, default is 3000.
  #
  port        ENV.fetch("PORT") { 3000 }

  # Specifies the `environment` that Puma will run in.
  #
  environment ENV.fetch("RAILS_ENV") { "development" }

  # Specifies the number of `workers` to boot in clustered mode.
  # Workers are forked webserver processes. If using threads and workers together
  # the concurrency of the application would be max `threads` * `workers`.
  # Workers do not work on JRuby or Windows (both of which do not support
  # processes).
  #
  workers ENV.fetch("WEB_CONCURRENCY") { 5 }

  # Use the `preload_app!` method when specifying a `workers` number.
  # This directive tells Puma to first boot the application and load code
  # before forking the application. This takes advantage of Copy On Write
  # process behavior so workers use less memory. If you use this option
  # you need to make sure to reconnect any threads in the `on_worker_boot`
  # block.
  #
  preload_app!

  # The code in the `on_worker_boot` will be called if you are using
  # clustered mode by specifying a number of `workers`. After each worker
  # process is booted this block will be run, if you are using `preload_app!`
  # option you will want to use this block to reconnect to any threads
  # or connections that may have been created at application boot, Ruby
  # cannot share connections between processes.
  #
  on_worker_boot do
    ActiveRecord::Base.establish_connection if defined?(ActiveRecord)
  end

  # Allow puma to be restarted by `rails restart` command.
  plugin :tmp_restart
  ```

- Create a file called `Procfile` at the root directory of your app and add the following:

  ```
    web: bundle exec puma -C config/puma.rb
  ```

- Next, let's add the following code into your gemfile:

  ```
    group :production do
      gem 'rails_12factor'
    end
  ```

- You can read about rails_12factor [here](https://github.com/heroku/rails_12factor)

- Run `git push heroku master` to push your application to heroku.

- You should see a long list of `jargon`. This is heroku's `buildpack` compiling and install your application for deployment.

- Once it's completed, run `heroku run rails db:migrate` to prepare your `online database`.

- Finally, run `figaro heroku:set -e production` to push your environment keys into heroku.
