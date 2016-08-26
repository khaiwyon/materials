# Omniauth Facebook

- You will be using [omniauth](https://github.com/mkdynamic/omniauth-facebook) to create a login function for your shopping app.

- Follow the documentation and set it up properly.

- There is one additional step that was not covered in the tutorial. You will need to die this in with a controller action to receive a hash containing their details from Facebook.

- In your `routes.rb` file, add:
  `get '/auth/:provider/callback', to: '<your-controller-name>#<your-controller-action>'`
