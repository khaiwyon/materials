# Day 4

## Email Reset

- We're now going to implement a password reset feature for the discussion board.

- Let's start by installing the [letter opener](https://github.com/ryanb/letter_opener) gem in your group `development`.

  ```
    group :development do
      gem 'letter_opener'
    end
  ```

- Next, in your `config/environments/development.rb` file, add or append the following if you already have it:

  ```
    config.action_mailer.delivery_method = :letter_opener
  ```

- This allows us to open emails sent to us from the server in development.

- We'll start by creating the `password_resets_controller.rb`

  ```
    class PasswordResetsController < ApplicationController

      def new
      end

      def create
        user = User.find_by(email: reset_password_params[:email])

        if user
          user.update(password_reset_token: password_token, password_reset_at: DateTime.now)
          PasswordResetsMailer.password_reset_mail(user).deliver_now
          flash[:success] = "We've sent you instructions on how to reset your password"
        else
          flash[:danger] = "User does not exist"
        end

        redirect_to new_password_reset_path
      end

      def edit
        @token = params[:id]
      end

      def update
        @user = User.find_by(password_reset_token: params[:id])

        if @user && token_active?
          @user.update(password: user_params[:password], password_reset_token: nil, password_reset_at: nil)
          flash[:success] = "Password updated, you may log in now"
          redirect_to root_path
        else
          flash[:danger] = "Error, token is invalid or has expired"
          render :edit
        end
      end

      private

      def reset_password_params
        params.require(:reset).permit(:email)
      end

      def user_params
        params.require(:user).permit(:password)
      end

      def password_token
        SecureRandom.urlsafe_base64
      end

      def token_active?
        (Time.now - @user.password_reset_at) < 24.hours
      end
    end
  ```

- `new` is the `html` page that contains a form that takes in a user's email. (The page to initialize a user's password reset)

- `create` does 3 things:
  - it creates a `password_token` which is a random string for the user.
  - it updates `password_reset_at` with the current time.
  - it calls `PasswordResetsMailer.password_reset_mail(user).deliver_now` which sends an email to the user's registered email.

- `edit` takes in the params (from the link of the email) and uses it to search for the user.

- `update` allows the user to set their new password. If a user is found and the criteria for `token_active` is met, it will allow the user to set
their new password. `token_active?` is a method that checks to see if the `password_reset_at` has exceeded 24 hours or not.

- Now let's create `new.html.erb` in `views/passwords`

  ```
  <div class="password-resets new">
    <div class="content">
      <%= form_for(:reset, url: password_resets_path, method: :post, html: { id: "user-reset-password-form" }) do |f| %>
        <h1 class="white shadowed text-center">
          Reset Password
        </h1>

        <div class="form-group">
          <h4> <%= f.label :email_field, "Email", class: "white shadowed" %> </h4>
          <%= f.email_field :email, required: true,  id: 'user_email_field', class: 'form-control' %>
        </div>

        <div class="form-group text-center">
          <%= f.submit "Reset my password!", class: "btn btn-primary btn-lg" %>
        </div>
      <% end %>
    </div>
  </div>
  ```

- REMEMBER TO USE YOUR OWN STYLING - THIS IS AN EXAMPLE. Note the `<%= form_for(:reset...)`. You can actually pass in a symbol instead of an object. `Rails` will use the name of that symbol
as the hash name instead of the object's name.

- We are using `reset` because we need to match the permitted parameters set in the `reset_password_params` method in the `passwords controller`.

- Now we'll build the `mailer`. Go to `app/mailers` folder and create a new file called `password_resets_mailer.rb`

  ```
    class PasswordResetsMailer < ApplicationMailer

      def password_reset_mail(user)
        @user = user
        @url = "#{ENV.fetch('SERVER_URL')}/password_resets/#{@user.password_reset_token}/edit"
        mail(to: @user.email, subject: "Your password has been reset")
      end
    end
  ```

- [Mailers](http://guides.rubyonrails.org/action_mailer_basics.html) are a `Rails` module that helps you with sending out emails.

- We're next going to create another folder in `app/views` called `password_resets`

- Inside, create a new file called `password_reset_mail.html.erb`

  ```
    <h5> <%= "Hey there, #{@user.username}" %> </h5>

    <p> You've request to reset your password, click the URL below to reset it! </p>
    <p> <%= @url %> </p>

    <p> Regards, </p>
    <p> Roti Telur Discussions </p>
  ```
