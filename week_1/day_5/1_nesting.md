# Day 5 Lessons

## Nesting Resources

- First let's remove `optional: true` from the `Post model`.

- Now, we're going to add structure to our application.

- As you've learnt, we've created `topics`, and we've created `posts`.

- We also know that `posts` are nested (belongs to) under `topics`.

- Let's start by nesting our `posts` routes under `topics`

- Go to your `routes` and type in the following:

  ```
    resources :topics, except: [:show] do
      resources :posts, except: [:show]
    end
  ```

- We've now nested our posts url under topics, if you type `rails routes`, you'll see:

  ![nested routes](images/nested_routes.png)

- We've also removed the `show url` from both `topics` and `posts`, we don't need them anymore

- Notice that your urls are now "/topics/*/posts"

- This is to allow scoping as each individual topics will have their own posts.

- On top of just passing our post's `id`, we now have to pass in a new parameter called
`topic_id` when dealing with `posts`.

- We now have quite a bit of refactoring to do for our `posts` and `topics` controllers.

- First and foremost, let's remove the `show` method and views from both controllers.

- Next, we are going to add new lines of code into your `PostsController` methods.

  ```
    class PostsController < ApplicationController

    def index
      @topic = Topic.includes(:posts).find_by(id: params[:topic_id])
      @posts = @topic.posts.order("created_at DESC")
    end

    def new
      @topic = Topic.find_by(id: params[:id])
      @post = Post.new
    end

    def create
      @topic = Topic.find_by(id: params[:topic_id])
      @post = Post.new(post_params.merge(topic_id: params[:topic_id]))

      if @post.save
        redirect_to topic_posts_path(@topic)
      else
        redirect_to new_topic_post_path(@topic)
      end
    end

    def edit
      @post = Post.find_by(id: params[:id])
    end

    def update
      @topic = Topic.find_by(id: params[:topic_id])
      @post = Post.find_by(id: params[:id])

      if @post.update(post_params)
        redirect_to topic_posts_path(@topic)
      else
        redirect_to edit_topic_post_path(@topic, @post)
      end
    end

    def destroy
      @post = Post.find_by(id: params[:id])

      if @post.destroy
        redirect_to topic_posts_path(@topic)
      end
    end

    private

    def post_params
      params.require(:post).permit(:title, :body)
    end
  end
  ```

- As mentioned, since we've nested posts under topics, we can now scope to show only posts relevant to the topic we've selected.

- Because of this, we have to pass the `topic` id into our `urls` well.

- EXAMPLE:

- Instead of `<%= link_to "Show", post_path(@post) %>`, it is now `<%= link_to "Show", topic_post_path(@topic, @post) %>`

- Similarly, for your forms. It is now `<%= form_for(@post, topic_post_path(@topic, @post) ... %>)`
