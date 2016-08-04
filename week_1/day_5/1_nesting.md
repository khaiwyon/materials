# Day 5 Lessons

## Nesting Resources

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

- Notice that your urls are now "/topics/*/posts"

- This is to allow scoping as each individual topics will have their own posts.


- On top of just passing our post's `id`, we now have to pass in a new parameter called
`topic_id` when dealing with `posts`.

- We now have quite a bit of refactoring to do for our `posts` and `topics` controllers.

- First and foremost, let's remove the `show` method from both controllers, we don't need them anymore.
