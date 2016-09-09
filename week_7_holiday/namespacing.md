# Namespacing

- Namespacing is an important aspect of development, it allows developers to properly organize their files to maintain readability.

- For your project, you will need to namepsace your `API Controllers` into `/api/v1` in order for the app to communicate with your back-end application.

- To start, we'll need to create a folder called `api` and 'api/v1' inside your `controllers folder`.

- Next, we'll need to modify `inflection.rb` inside your `config/initializers` folder to accept `API` as `api` for your namespacing. Type the code below to replace whatever is inside:

  ```
    ActiveSupport::Inflector.inflections(:en) do |inflect|
      inflect.acronym 'API'
    end
  ```

- [Inflectors](http://api.rubyonrails.org/classes/ActiveSupport/Inflector.html) allows us to transform (in this case) lower cased letter (api) to upper cased letters (API).

- In our routes, we'll also need to [namespace](http://guides.rubyonrails.org/routing.html#controller-namespaces-and-routing) it in order to achieve `/api/v1`. We can use it this way:

  ```
  namespace :api do
    namespace :v1 do
    end
  end
  ```
