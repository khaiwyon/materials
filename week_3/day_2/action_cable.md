# Day 2

## ActionCable

- Welcome to [Web Sockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API).

- To summarize, web sockets allows a more interactive experience for your user.

- Regular HTTP only updates information on the page when you refresh the page. With web sockets, it is updated in real time. This is especially
important for applications such as chat

- Let's start by installing [redis](https://github.com/redis/redis-rb) gem.

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
        setTimeout(
          if $('.comments.index').data().id == data.post_id && $(".comment[data-id=#{data.comment_id}]").length < 1
            $('#comments').append(data.partial)
        , 100)

    $(document).on 'turbolinks:load', postsChannelFunctions
  ```

- This is the front-end implementation of it.

- Note that the channel name must match the `back-end class name`

- `connected()` is the callback function when you have successfully established a cable connection.

- Likewise, `disconnected()` is the callback when you're disconnected.

- `received(data)` is the callback with the parameter `data` when `rails broadcasts` a new `message` to the appropriate `channel`
