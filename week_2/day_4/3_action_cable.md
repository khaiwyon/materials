# Day 2

## ActionCable

- Welcome to [Web Sockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API).

- To summarize, web sockets allows a more interactive experience for your user.

- Regular HTTP only updates information on the page when you refresh the page. With web sockets, it is updated in real time. This is especially
important for applications such as chat

- Let's start with installing `redis` server on your machine:
  - Mac users can install it by typing in their terminal `brew install redis`
  - Ubuntu users can install it by following this [guide](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)

- Next, we'll need to install [redis](https://github.com/redis/redis-rb) gem.


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

    if $('.comments.index').length > 0
      App.posts_channel = App.cable.subscriptions.create {
        channel: "PostsChannel"
      },
      connected: () ->

      disconnected: () ->

      received: (data) ->
        if $('.comments.index').data().id == data.post_id && $(".comment[data-id=#{data.comment_id}]").length < 1
          $('#comments').append(data.partial)

    $(document).on 'turbolinks:load', postsChannelFunctions
  ```

- This is the front-end implementation of it.

- Note that the channel name must match the `back-end class name`

- `connected()` is the callback function when you have successfully established a cable connection.

- Likewise, `disconnected()` is the callback when you're disconnected.

- `received(data)` is the callback with the parameter `data` when `rails broadcasts` a new `message` to the appropriate `channel`

- Now that we've using `ActionCable` to generate our comments, we don't need to do it in our `create.js.erb` anymore. Go ahead and remove this part:
  ```
    $('#comments').append("<%=j render partial: 'comments/comment', locals: { comment: @comment, post: @comment.post } %>")
  ```

- `$('.comments.index').data().id == data.post_id` checks to make sure that the user is in the appropriate post page before the comment is appended to the page.

- `$(".comment[data-id=#{data.comment_id}]").length < 1` is implemented to prevent redundancy (because you've already appended it in `create.js.erb` file).

- Finally, we're going to add an [ActiveJob](http://guides.rubyonrails.org/active_job_basics.html) to broadcast the new comment to anyone who is in the post itself.

- Create a new file called `comment_broadcast_job.rb` and paste in the following:

  ```
  class CommentBroadcastJob < ApplicationJob
    queue_as :default

    def perform(comment, user)
      ActionCable.server.broadcast 'posts_channel', comment: comment, post: comment.post, partial: render_comment_partial(comment, user)
    end

    private

    def render_comment_partial(comment, user)
      CommentsController.render partial: "comments/comment", locals: { comment: comment, post: comment.post, current_user: user }
    end
  end
  ```

- In reference to the documentation, `perform` is the method that `rails` will use to take action on.

- `render_comment_partial` is a private method we've built to create the `html string` that we will pass to our `js` side later.

- `ActionCable.server.broadcast` will send the following json to anyone who is subscribed to the `posts_channel`. In this case, we are sending in
  `{ comment: <comment>, post: <post>, partial: '<div></div>' }`

- If you want to find out more about what you actually get, you can type debugger under `received(data)` and type a comment with `chrome debugger` open.

## A little fun with Notifications

- Let's have a little fun and build a notification system if the user is not zoomed on the page.

- Create a `coffee` file called `notifications_permission.coffee`

  ```
    notificationPermission = () ->
    Notification.requestPermission()

    $(document).on 'turbolinks:load', notificationPermission
  ```

- This is the function to request for permission from the user to allow notifications to be displayed on the browser.

- Next, we're going to add extra code into your `received(data)` function.

  ```
  received: (data) ->
  setTimeout(
    if $('.comments.index').data().id == data.post.id && $(".comment[data-id=#{data.comment.id}]").length < 1
      $('#comments').append(data.partial)

      if document.hidden
        notification = new Notification data.post.title, body: data.comment.body, icon: data.post.image.thumb.url

        notification.onclick = () ->
          window.focus()
          this.close()
  , 100)
  ```

- `document.hidden` checks to see if the user is on the current tab / browser or not.

- You can find out more about `Notification` [here](https://developer.mozilla.org/en/docs/Web/API/notification)

- Finally, to implement this, add the following method into your `comments controller - create` method:

  ```
  if @comment.save
    flash.now[:success] = "Comment created"
    CommentBroadcastJob.perform_later(@comment, current_user, "create")
  else
    flash.now[:danger] = @comment.errors.full_messages
  end
  ```

- `CommentBroadcastJob.perform_later(@comment, current_user, "create")` will trigger `Rails` to broadcast your new comment to any other user who is visiting that `post` as well.
