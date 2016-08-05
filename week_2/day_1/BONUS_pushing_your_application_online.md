## Uploading to HEROKU

- Warning: You will need a credit card for this. The service is free if you choose their free servers, however a card is required for verification.

- Install your heroku toolbelt [here](https://toolbelt.heroku.com/)

- Referring this [documentation](https://devcenter.heroku.com/articles/creating-apps)

- On the directory of your app, type `heroku create <your-app-name>`.

- You should see heroku creating a new application for you.

- You can check it on your heroku dashboard (www.heroku.com)

- Run `git push heroku master` to push your application to heroku.

- You should see a long list of `jargon`. This is heroku's `buildpack` compiling and install your application for deployment.

- Once it's completed, run `heroku run rails db:migrate` to prepare your `online database`.

- Finally, run `figaro heroku:set -e production` to push your environment keys into heroku.
