# Omniauth Facebook

- You will be using [omniauth](https://github.com/mkdynamic/omniauth-facebook) to create a login function for your shopping app.

- Follow the documentation and set it up properly.

- There is one additional step that was not covered in the tutorial. You will need to die this in with a controller action to receive a hash containing their details from Facebook.

- The user flow for omniauth:-
  - Search for the user using `uid` and `email` if uid fails.
  - If a user is found via `email`, update the user with the `uid` acquired from facebook.
  - If a user is not found, create a new user and log the user in.

- In your `routes.rb` file, add:
  `get '/auth/:provider/callback', to: '<your-controller-name>#<your-controller-action>'`

- Be aware that you will need to save the user's `uid` and `provider` in the User Model.
