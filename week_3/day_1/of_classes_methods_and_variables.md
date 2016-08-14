## Of Classes, Methods, and Variables

- Example:
  ```
  class Numbers
    @@four = "Four"
    @@six = "Six"

    def initialize
      @one = "One"
      two = "Two"
      years = 25
      @job = "Developer"
      @five = "Five"
    end
    attr_reader :job

    def one
      @one
    end

    def two
      two
    end

    def three
      three = "Three"
      three
    end

    def four
      @@four
    end

    def self.five
      @five
    end

    def self.six
      @@six
    end

    def self.set_seven
      @seven = "Seven"
    end

    def self.run_seven
      @seven
    end
  end
  ```

- An instance variable is available to a class. It is marked with `@`. An instance if a class is created
when you use `.new`. For example, the class above is called `Numbers`. Calling `Numbers.new` will instantiate it and make the variable
`@name` available to the whole class. Hence calling:
  ```
  x = Numbers.new
  x.one
  ```
  will output "One".

- A regular variable, unlike instance variables, is scoped within the method only. For example, the variable `years` inside the method `initialize`, which
is called when the class is instantiated - is only available within that method. Hence calling `breaking_age` will return an error.

- A class variable is is marked with 2 `@`s. This variable - similar to the instance variable is also available in the entire Class itself. There is however
a slight difference, class variables are immediately available to the class without having to be instantiated or initialized through a method call first.

## How does this apply to Rails?

- We use `instance variables` in our controller actions (methods) because Rails actually has an implicit render built into it.

- The file is [here](https://github.com/rails/rails/blob/master/actionpack/lib/action_controller/metal/basic_implicit_render.rb):
  ```
  module ActionController
    module BasicImplicitRender # :nodoc:
      def send_action(method, *args)
        super.tap { default_render unless performed? }
      end

      def default_render(*args)
        head :no_content
      end
    end
  end
  ```

- This piece of code is simple - every time `send_action` (which is the method by the routes) is performed,
it checks to see if render has been explicitly called or not - if it has not - it will then implicitly call it following conventions.

- `render` is a method available inside the `ActionController` module - and as you have known - if you need data to be accessible by another method
in the same class - it needs to be wrapped inside a method and called or be available as an `instance variable`.
