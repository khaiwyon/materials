# Day 4

## Authorization

- Now that' we've build our authentication module, it's time to add authorization.

- Note that if you did not add a `role default` to `0`, you can set it now with generating a rails migration file.

- `rails g migration AddDefaultToUserRole`

- Open your newly generated migration file and add in:

  ```
    def change
      change_column :users, :role, :integer, default: 0
    end
  ```

- We're going to have 3 different user roles - the admin, the moderator, and the user.

- Let's add this in. Remember that we've already added the attribute `role:integer` in our `users table`

-

- In your user model, add:

  ```
    class User < ApplicationRecord

      <!-- Your code here -->
      enum role: [:user, :moderator, :admin]
    end
  ```

- This enumerates your user's `role` column:
  - 0 sets it to user
  - 1 sets it to moderator
  - 2 sets it to admin

- The migration that you just ran sets it to 0 (which is default for `user`)

- Once done, it's time to install [pundit](https://github.com/elabs/pundit)

- Pundit is an authorization library that checks for various requirements that you set before it allows the user to access or perform the action. Remember to bundle.

- Time to integrate `Pundit`

- In your `application_controller.rb`, add the following code:

  ```
    class ApplicationController < ActionController::Base
      include Pundit
      protect_from_forgery with: :exception

      rescue_from Pundit::NotAuthorizedError do |exception|
        flash[:danger] = "You're not authorized"
        redirect_to request.referrer || root_path
      end

      <!-- your other rails code -->
    end
  ```

- `include Pundit` is self-explanatory.

- `rescue_from` method is called when Pundit raises an authorization error to your user. In this case, it raises a `flash message` and `redirects` the user to the previous page or to the root path.

- Next, let's create a new folder under `app` called `policies`

- Inside `app/policies`, create a new file called `application_policy.rb`

- Paste the following code:

  ```
    class ApplicationPolicy
      attr_reader :user, :record

      def initialize(user, record)
        @user = user
        @record = record
      end

      def scope
        Pundit.policy_scope!(user, record.class)
      end
    end
  ```

- Now we're going to work on `topics` first. Create another new file called `topic_policy.rb`. (Naming convention - policies adhere to singularity, like `models`). E.G `PostPolicy`, `UserPolicy`..


  ```
    class TopicPolicy < ApplicationPolicy

    def new?
      user.present? && user.admin?
    end

    def create?
      new?
    end

    def edit?
      new?
    end

    def update?
      new?
    end

    def destroy?
      new?
    end
  end
  ```

- The name of the action follows the actions in the controller with an appended `?`. For example, `new` in your topics controller will create `new?` inside your policy.

- We have to checks inside that method - it first checks for whether the user is present before checking if the user is an admin. If both requirements are met, it will allow the user to perform the function, otherwise
it will render an error.

- Notice now that other methods use the method `new?`, this essentially calls `user.present? && user.admin?` instead having to type it all over again.

- In summary, we've set that while anyone can view (index) the topics, creating, editing, and deleting topics are only restricted to admins.

- Finally, inside your topics controller, we can call the policy by adding the method `authorize`, like so:

  ```
  class TopicsController < ApplicationController
    <-- Some other code -->

    def new
      @topic = Topic.new
      authorize @topic
    end

    <-- Some other code -->
  end
  ```

- `current_user` is automatically passed into pundit if it is declared so you don't have to worry about passing in the `user` object.
