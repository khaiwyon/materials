# Day 2

## ActionCable

- Welcome to [Web Sockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API).

- To summarize, web sockets allows a more interactive experience for your user.

- Regular HTTP only updates information on the page when you refresh the page. With web sockets, it is updated in real time. This is especially
important for applications such as chat

- Let's start with installing `redis` server on your machine:
  - Mac users can install it by typing in their terminal `brew install redis`
  - Ubuntu users can install it by following this [guide](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)

- We'll also need to install [redis](https://github.com/redis/redis-rb) gem.

- Go to your `routes.rb` file and add the following line:
  ```
    mount ActionCable.server => '/cable'
  ```

- Next, open your `config/cable.yml` and change the following to this:

  ```
  development:
    adapter: redis

  test:
    adapter: redis

  <-- production code -->
  ```

- Next, we're going to create a new file inside `app/channels` called `posts_channel.rb`

  ```
  class PostsChannel < ApplicationCable::Channel

    def subscribed
      stream_from "posts_channel"
      logger.add_tags 'ActionCable', "User connected to posts channel"
    end

    def unsubscribed
    end
  end
  ```

- This is the back-end implementation of `action cable`

- Now we're going to implement the front-end, create a new file inside `assets/javascripts/channels` called `posts_channel.coffee`

  ```
    postsChannelFunctions = () ->

    checkMe = (comment_id, username) ->
      unless $('meta[name=admin]').length > 0 || $("meta[user=#{username}]").length > 0
        $("<your-comment-element-name>[data-id=#{comment_id}] .<your-buttons-container-element>").remove()
      $("<your-comment-element-name>[data-id=#{comment_id}]").removeClass("hidden")

    if $('<your-comments-index-element>').length > 0
      App.posts_channel = App.cable.subscriptions.create {
        channel: "PostsChannel"
      },
      connected: () ->

      disconnected: () ->

      received: (data) ->
      if $('<your-comments-index-element>').data().id
        $('<your-comments-container>').append(data.partial)
        checkMe(data.comment.id)

    $(document).on 'turbolinks:load', postsChannelFunctions
  ```

- This is the `Javascript` implementation of it.

- Note that the channel name must match the `back-end class name` - `channel: "PostsChannel"`

- `connected()` is the callback function when you have successfully established a cable connection.

- Likewise, `disconnected()` is the callback when you're disconnected.

- `received(data)` is the callback with the parameter `data` when `rails broadcasts` a new `message` to the appropriate `channel`

- The partial rendered is by default hidden when it is rendered from the back-end (I will explain it clearly in a few moments).

- TO NOTE:
  ```
    checkMe = (comment_id, username) ->
      unless $('meta[name=admin]').length > 0 || $("meta[user=#{username}]").length > 0
        $("<your-comment-element-name>[data-id=#{comment_id}] .<your-buttons-container-element>").remove()
      $("<your-comment-element-name>[data-id=#{comment_id}]").removeClass("hidden")
  ```

- `checkMe` is a custom function we've added to check if the user that is logged in is an admin or the comment's owner or not - if they are not,
we will remove the `<your-buttons-container-element>` div to remove the edit and delete buttons from the user. The check is seen here:
```unless $('meta[name=admin]').length > 0 || $("meta[user=#{username}]").length > 0```

- This is the `meta` that is rendered on your `<head>` tag inside your `application.html.erb`:
  ```
  <% if current_user %>
    <meta user="<%=current_user.username%>">
    <% if current_user.admin? || current_user.moderator? %>
      <meta name="admin">
    <% end %>
  <% end %>
  ```

- This is fairly simple, if a user is logged in render their username as a `meta tag`, and if they are an admin, render `meta tag` `admin`

- Next, we'll add `data-id` to your `comments index` page to uniquely identify your each of your post. For example:
  ```
    <div class="your-comments-index" data-id="<%=@post.id%>">
      <-- your html code -->
    </div>
  ```

- You should do the same for your `_comment.html.erb` partial if you have not:
  ```
    <div class="comment" data-id="<%=comment.id%>">
      <-- your html code -->
    </div>
  ```

- Now that we've implemented the front-end, let's go back to the back-end on how to broadcast this.

- Broadcasting Comments is done using [ActiveJob](http://guides.rubyonrails.org/active_job_basics.html). ActiveJobs is a framework that allows the application
to `queue` jobs in the backend. These jobs are usually run in parallel by another machine - thus keeping the main application performant by delegating it's workload to other
"cpus" in the server.

- Let's do this by creating a new file called `comment_broadcast_job.rb` inside `jobs`.

  ```
  class CommentBroadcastJob < ApplicationJob
    queue_as :default

    def perform(type, comment)
      ActionCable.server.broadcast 'posts_channel', type: type, comment: comment, post: comment.post, username: comment.user.username, partial: render_comment_partial(comment)
    end

    private

    def render_comment_partial(comment)
      CommentsController.render partial: "comments/comment", locals: { comment: comment, post: comment.post, current_user: comment.user, extra_class: "hidden" }
    end
  end
  ```

- `ActionCable.server.broadcast` inside `perform` is the method `ActionCable` class uses to broadcast a specific message to a channel, which is then sent to the `JS`
implementation on the front-end.

- `render_comment_partial` is the method used to render the comment partial. As the render method is inside the Controller Class - we'll need to declare `CommentsController` in order
to render it. The rest is fairly similar to what you would call in the `views`.

- Note that `extra_class` is the value passed in for `<div class="comment <%=extra_class%>" data-id="<%= comment.id %>">` in the comment partial to first render it to the user hidden. Remember to edit your partials to allow
this value to be passed.

- In this case, I've passed in the value `hidden` to the partial to make sure it's hidden first.
This is to allow the check by JS as seen inside the `checkMe` function to trim the `control-panel` element if the user is not an admin before revealing it to the user.

- CSS for hidden:
  ```
  .hidden {
    display: none;
  }
  ```

- Now to call the function inside your comments controller. Inside your `create` method, add `CommentBroadcastJob.perform_later("create", @comment)` after `@comment.save`:

  ```
    def create
    @comment = current_user.comments.build(comment_params.merge(post_id: params[:post_id]))
    @new_comment = Comment.new
    @post = Post.find_by(id: params[:post_id])

    if @comment.save
      CommentBroadcastJob.perform_later("create", @comment)
      flash.now[:success] = "Comment created"
    else
      flash.now[:danger] = @comment.errors.full_messages
    end
  end
  ```

- Finally, remove your `append` or `prepend` javascript function from your `create.js.erb` because you don't need it anymore.

## Challenge:

- Build an implementation for edit and destroy.

- HINTS:

- User `debugger` in your `javascript` files. `debugger` works similar to `binding.pry`

- Notice the argument `type` in `CommentBroadcastJob` - you can use to the pass the action type to your broadcast job so your js will know what to do for each comment action type.

- Example:
  ```
  received: (data) ->
  switch data.type
    when "create" then createComment(data)
    when "update" then updateComment(data)
    when "destroy" then destroyComment(data)
  ```

- This is a simple switch that performs the appropriate function depending on what data.type is - which is send from `broadcast` in your comments job.

- You will need to use `perform_now` instead of `perform_later` for `comments destroy` in your controller.
