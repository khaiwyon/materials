# Day 2 Lessons

## Authenticating Users

- In the first part, you add `has_secure_password` to your User model.

- This allows Rails to use `bcrypt` library to securely store your users' password.

- For security reasons, we do not want to save the users' actual password into the database. What `bcrypt` does is that it hashes it and saves a `digital fingerprint` into your database.
Hash passwords are `irreversible`, there is no way of it going back into the actual `password`. When a user keys in their password again for verification, it just hashes the password into
the `salt` key and compares it to make sure the two are the same. In summary, your actual password is never stored or used to compare - only a hashed version of it.

- Bonus Reading Material:
  - [#1](http://dustwell.com/how-to-handle-passwords-bcrypt.html)
  - [#2](http://stackoverflow.com/questions/5393803/can-someone-explain-how-bcrypt-verifies-a-hash)
  - [EKSBlowfish Algorithm](https://www.usenix.org/legacy/publications/library/proceedings/usenix99/full_papers/provos/provos_html/node4.html)

- Now that we've set up `bcrypt`, we're going to create a `sessions` controller.

  ```
    class SessionsController < ApplicationController

    def new
    end

    def create
      user = User.find_by(email: user_params[:email])
                 &.authenticate(user_params[:password])

      if user
        session[:id] = user.id
        flash[:success] = "Welcome back #{current_user.username}"
        redirect_to root_path
      else
        flash[:danger] = "Error logging in"
        render :new
      end

    end

    def destroy
      session.delete(:id)
      flash[:success] = "You've been logged out"
      redirect_to root_path
    end

    private

    def user_params
      params.require(:user).permit(:email, :password)
    end
  end
  ```

- The `new` action is your login page, where there would be a form for your user to key in their credentials. `Create` is the action that finds your user using `email` in the database,
authenticates them by comparing their password with the hash and saving their id in a session if a match is found.

- `Destroy` is to remove the `id` session, thus logging the user out.

- Create a new `resource` in your routes that only has `new`, `create`, and `destroy`

- Next, we're going to add a new `method` in your `applicaton_controller.rb`

  ```
    class ApplicationController < ActionController::Base
    protect_from_forgery with: :exception

    private

    def current_user
      return unless session[:id]
      @user ||= User.find_by(id: session[:id])
    end
    helper_method :current_user
  end
  ```

- [helper_method](http://apidock.com/rails/AbstractController/Helpers/ClassMethods/helper_method) is a rails controller that declares a controller method is a `helper`.
This allows that `method` to also be called in the `view`. With this, our controllers and views can now call the method `current_user` that fetches the current signed user.

- Rails [sessions](http://guides.rubyonrails.org/security.html#sessions) allows you to store secure cookies in your browser. Among this is your user's login session, to let
your application know that your user has logged in.

- `authenticate` is a `bcrypt` gem method that allows you to compare the password sent by the user with the hash in your database. If it matches, it will authenticate the user
and send in a `session` containing the user's `id`

- `&.` before `authenticate` is a ruby [safe navigator](http://mitrev.net/ruby/2015/11/13/the-operator-in-ruby/) that returns a nil if the method does not exist in the object.
In this case, if a user is not found, the method `authenticate` will not be available to a `nil` value. Hence a safe navigator is used to ensure this does not raise an error and break
the app.

- Next, let's add this method in below `private` in your `application controller`

  ```
    def authenticate!
    unless current_user
      redirect_to root_path
      flash[:danger] = "You need to login first"
    end
  end
  ```

- This is a method that redirects users to the root path of your app if they are not logged in.

- To call this, we're going to add it in our controllers for actions that require the user to log in first, for example: creating a new post.

- Using `topics controller` as the example, let's add a `before_action` [filter](http://guides.rubyonrails.org/action_controller_overview.html#filters)

  ```
  class TopicsController < ApplicationController
    before_action :authenticate!, only: [:create, :edit, :update, :new, :destroy]

    <-- Your Controller Code... -->
  end
  ```

- This tells your rails application to run the `authenticate!` method from your `application controller` first before running the action.

- The reason why the method is available to your `topics` controller is because it inherits all methods available in its parent - the `application controller`, marked by the following syntax:
  ``` class TopicsController < ApplicationController ```
