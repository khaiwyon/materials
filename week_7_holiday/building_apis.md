# Setting Up

- We'll need to set up to allow [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

- Install this [gem](https://github.com/cyu/rack-cors).

- Create a new initializer file called `rack_cors.rb`.

  ```
  Rails.application.config.middleware.insert_before 0, Rack::Cors do
    allow do
      origins '*'
      resource '*', headers: :any, methods: [:get, :put, :delete, :post, :options]
    end
  end
  ```

- This will be enough for your project, what this does is that we'll allow anyone to access our api. Actual production apps may require certain credentials, but we'll leave that for now.

# Building APIs

- The Rails framework allows developers to quickly craft APIs that other apps or servers interact with. The design of creating these APIs are very similar to building a regular APP. All API endpoints interact with `controllers`.

- The only difference is that instead of rendering HTML templates, you will need to render in [JSON](https://en.wikipedia.org/wiki/JSON) format.

- To use this, we will be using the `render(json: {}, status: 200)` format, you can find the documentation [here](http://guides.rubyonrails.org/layouts_and_rendering.html#using-render).

- HTTP Statuses can be referred [here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Building JSON endpoints:
  - [BYO](http://api.rubyonrails.org/classes/ActiveSupport/JSON.html)
  - [Rails JBuilder](https://github.com/rails/jbuilder)
  - [Active Model Serializers](https://github.com/rails-api/active_model_serializers)
  - [Amatsuda's JB](https://github.com/amatsuda/jb)

- You're free to use any of these ways to build your app, the tutorial I will be providing will use Active Model Serializers.

## Using Active Model Serializers

- Referring to the [documentation](https://github.com/rails-api/active_model_serializers/blob/master/docs/general/getting_started.md), quickly generate a serializer you need by typing in the `generator` in your `terminal`.

- In this case, I'll type `rails g serializer task` to generate a `Task Serializer`

- This would generate a file inside your `serializers` folder:
  ```
  class TaskSerializer < ActiveModel::Serializer
  end
  ```

- To pass in the attributes we want, we'll need to add the `attributes` method in the class.

- For example, adding `attributes :description` will generate a JSON of the task with the data `description` inside it.

  ```
  class TaskSerializer < ActiveModel::Serializer
    attributes :description
  end
  ```

- Finish up this serializer based on the requirements of the project.

- HINT:
  - You should create your api folders inside `controllers/API` folder. You will then need to nest it as well based on the folder name. In this case, since the folder is called `API`, you would name it `API::LandingController`.
