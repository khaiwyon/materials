## Inheritance

- Inheritance is when a class "inherits" the implementation / behaviour from a parent class.

- Example:

  ```
    class Person

      class << self
        def profession(profession)
          profession
        end

        def name(name)
          name
        end

        def age(age)
          age
        end
      end
    end

    class Bob < Person
    end
  ```

- Note: ```class << self ... end ``` sets all your methods inside it as `class methods (remember: self)`

- In the example, the class `Bob` inherits off the `Person` class, and will be able to use all the methods defined
inside `Person` class.

- `Bob.age(25)` will return the number 25 - as defined in the `Person` class.

- Inheritance is a great way to reuse your code - as seen in your `rails` application - for example: the `controllers`,
in which they all inherit from your `application_controller.rb` which itself inherits from `ActionController::Base`.

## How does this apply to Rails?

- Rails simplifies the interface by 'hiding' most of it's implementations. A great way of doing this is through inheritance.
It also allows code to be reused over and over again without having to retype it. If inheritance did not exist, you would have
to type thousands of code every time you built a new controller.

- This is why you see ApplicationController inherits from ActionController::Base, which itself loads from a huge series of
modules and classes.
