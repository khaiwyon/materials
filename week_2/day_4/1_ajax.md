# Day 3

## Going AJAX

- WARNING: COPY PASTING AT THIS LESSON MAY NOT WORK BECAUSE YOUR APP MAY BE STYLED DIFFERENTLY, FOLLOW THE EXAMPLE AND CUSTOMIZE ACCORDING TO YOUR APP.

- [Ajax](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started) is the acronym for "Asynchronous Javascript And XML". In simple terms,
AJAX's greatest appeal is the ability to make a server-side call without refreshing the page itself.

- Let's start by first adding `respond_to :js` at the top of your `comments_controller`. If it isn't working, add the `gem 'responders'` into your gemfile.

  ```
  class CommentsController < ApplicationController
    respond_to :js
    before_action :authenticate!, except: [:index]

    <--- your remaining code --->
  end
  ```

- [respond_to](http://apidock.com/rails/ActionController/MimeResponds/InstanceMethods/respond_to) is a rails controller
method that sets what sort of format the action should respond to on top of the default `http`.

- We'll now move the form from `comments new` and it's associated styling to comments index.

- We're next going to remove `new` from comments. Go ahead and delete `comments/new.html.erb` and remove `new` method
from the `controller`.

- PRE-INFO: ALL YOUR PARTIALS SHOULD BE IN THE `COMMENTS` FOLDER:

- Now, we're going to move the html in your `@comments` loop into a partial:

- For example:

  ```
    <div id="comments">
      <% @comments.each do |comment| %>
        <%= render partial: "comment", locals: { comment: comment, post: @post } %>
      <% end %>
    </div>
  ```

- `locals` is how we can pass variables into a partial.

- Finally, we're going to create a `create.js.erb` file in your `views/comments/

- A `js.erb` file is a `javascript file with embedded ruby`. This allows you to call both ruby and javascript code.

- Rails detects `js.erb` files with the same name as the controller `action` and calls it if you enable `respond_to :js`

- We need to do three things when a user creates a new comment:
  - Reset the form so a new comment can be posted.
  - Append the bottom of the comments index with the new comment.
  - Render the flash message.

- Let's do the easiest one first. Since we're no longer refreshing the page if a new comment is posted, we'll need to change the flash a little.

- Instead of just `flash[:success] = <your-message>`. We have to call `flash.now[:success] = <your-message>`

- Similar to our `comments` partial, we are now going to wrap our comments form into a container and move it into a partial:

  ```
    <div id="comments-form-container">
      <%= render partial: "form", locals: { comment: @comment, post: @post } %>
    </div>
  ```

- Add a new parameter called `remote: true` into your `comments form` as well. Example:
  ```
    <%= form_for(comment, url: topic_post_comments_path(post.topic, post), method: :post, remote: true, html: { id: "comment-create-form" }) do |f| %>
      <!-- your form code -->
    <% end %>
  ```

- Finally, we'll need to do a little refactoring. Previously, you would use `instance variables` such as `@comment`, `@topic` or `@post`. This applies slightly differently in a `partial`.
`Partials` that are rendered through a controller action - `new` for example will be able to render it properly because the required `instance variables` are loaded from the controller. This is different
when you run `render partial`. The action does not go through the `controller` in a traditional sense, hence the `instance variable` will not be available. This is why we have to pass it through `local: {}`.

- For example, if your partial requires `@post`, you will need to turn it into `post` and pass it through `render partial`.
  ```
    render partial: "<your-partial-name>", locals: { post: @post }
  ```

- Now, let's create our `js.erb file` called `create.js.erb`. You may need to configure this based on how you've styled your application:

  ```
    $('#flash-messages-container').html("<%=j render partial: 'shared/flash_messages' %>")

    <% if @comment.persisted? %>
      $('#comments').append("<%=j render partial: 'comments/comment', locals: { comment: @comment, post: @comment.post } %>")
      $('#comments-form-container').html("<%=j render partial: 'comments/create_form', locals: { comment: @new_comment, post: @post } %>")
    <% end %>
  ```

- We'll now have to make a check on `@comment`. `persisted?` is a method that checks to see if `@comment` is in the database - hence it was successfully saved. If it was successfully saved,
we'll then render partial, otherwise we'll omit it.

- We'll also make the necessary adjustment to our controller to allow `@new_comment`

- EXAMPLE of my `create` method:
  ``
  def create
    @comment = current_user.comments.build(comment_params.merge(post_id: params[:post_id]))
    @new_comment = Comment.new
    @post = Post.find_by(id: params[:post_id])

    if @comment.save
      flash.now[:success] = "Comment created"
    else
      flash.now[:danger] = @comment.errors.full_messages
    end
  end
  ```

- Note that we now do not need `redirect_to` because we no longer need the page to refresh itself to reflect our newest changes.

- The first `javascript` function `html` is to replace any `html` inside your `comments-form-container` with the content set inside the `()`

- [<%=j %>](http://apidock.com/rails/ActionView/Helpers/JavaScriptHelper/escape_javascript) or `escape_javascript` is a rails helper method that allows you
to pass in ruby code as `html string` in `javascript`.

## Allowing image upload in AJAX.

- First, add `gem 'remotipart', github: 'mshibuya/remotipart', ref: '88d9a7d' into your `gemfile`.

- Next, open you `application.js` file and add `//= require jquery.remotipart` under `jquery_ujs`

- Finally, add `authenticty_token: true, and html: { multipart: true }` to you forms.

- EXAMPLE:
  ```
  <%= form_for(comment, url: topic_post_comments_path(post.topic, post), method: :post, authenticity_token: true, remote: true, html: { id: "comment-create-form", multipart: true }) do |f| %>
  ```
