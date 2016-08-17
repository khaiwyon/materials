# Model Methods

- In rails, model methods are a great way to provider custom data to your objects.

- Yesterday in your voting challenge attempt, you had to build your first model instance method `total_votes`, which tallies all your votes and returns a total.

- EXAMPLE usage:
  - You have `first_name` and `last_name` for your `User` model. If you want the full name, you can do something like this:

  ```
    class User < ApplicationRecord

      def full_name
        first_name + last_name
      end
    end
  ```

- NOTE that self is called implicitly in ruby, in other languages (Python for example) it is actually like this:
  ```
    def full_name
      self.first_name + last_name
    end
  ```

- calling `self` fetches the instance object that is calling this method. For example, `c = Comment.first` is an instance object of the `Comment` class.

- If you pry the `full_name` method and typed in `c.full_name`. You can call the object `c` by typing self while pried inside `full_name` method.``
