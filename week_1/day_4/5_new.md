## New Action

- Now we're going to learn how to add new topics into your database.

- First let's create a new view inside `views/topics` called `new.html.erb`

- Let's create a [form](http://guides.rubyonrails.org/form_helpers.html#binding-a-form-to-an-object) for you to create new topics with.

- Paste the following html into your `new.html.erb`

  ```
  <%= form_for(@topic, url: topics_path, html: { id: "topic-create-form" }) do |f| %>
    <h1 class="text-center">
      Create Topic
    </h1>

    <div class="form-group">
      <h4> <%= f.label :title_field, "Title" %> </h4>
      <%= f.text_field :title, required: true, id: "topic_title_field", class: 'form-control' %>
    </div>

    <div class="form-group">
      <h4> <%= f.label :text_field, "Description" %> </h4>
      <%= f.text_field :description, id: "topic_description_field", class: 'form-control' %>
    </div>

    <div class="form-group text-center">
      <%= f.submit "Create Topic", class: "btn btn-success btn-lg" %>
    </div>
  <% end %>
  ```

- This will be explained in a while, but let's add the following code into your `new` action in your `topics_controller.rb`

  ```
  def new
    @topic = Topic.new
  end
  ```

- `.new` is a method which initializes (but not saved) a new topic record. This record is then passed to your `new.html.erb` as an object to be used by your form.

- For the css stylings for your form, refer to [bootstrap](http://getbootstrap.com/css/#forms)

- `form_for` is a rails method for generating forms, `@topic` is the object that was passed to your view from the `new` action in your topics controller.

- `url: topics_path` sets the path where the data from the form will be sent to when you hit the submit button.

- `title` and `description` are the two attributes (refer to the model lessions) that your topic accepts.

- Once you've created it, go ahead and add `binding.pry` into your `create` method in your topics controller.

- Fill in your form and hit submit, `pry` should fire again in your terminal. Type `params` and you should be able to see the data that have been sent from your form.

- Finally, let's add a new button to your topics index page. Give it a try.

### FURTHER READING:

[Forms Generator](http://guides.rubyonrails.org/form_helpers.html)
