## Edit Action

- Now that you've learnt how to create topics, let's learn how to update them.

- First, add an `edit` button on your `index.html.erb` for each `topic`, passing the parameter `topic` as the id.

- In your `TopicsController`, add the following code into your `edit` method.

  ```
  class TopicsController < ApplicationController

    def edit
      @topic = Topic.find_by(id: params[:id])
    end
  end
  ```

- We'll need to create a new page called `edit.html.erb` on your `views/topics` folder. Paste the following form into it:

  ```
  <%= form_for(@topic, url: topic_path(@topic), html: { id: "topic-update-form" }) do |f| %>
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
      <%= f.submit "Update Topic", class: "btn btn-success btn-lg" %>
    </div>
  <% end %>
  ```
