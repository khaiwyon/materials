## Show Action

- The next action we're creating is the `show` action. Show takes the id as a parameter and fetches a specific record from the database.

- Let's add the following code

  ```
  @topic = Topic.find_by(id: params[:id])
  ```

- `params[:id]` is the parameter is retrieved from the url. For example, `localhost:3000/topics/1` will get you the `params[:id]` of `1`.

- Give it a try, add `binding.pry` inside your `show` method and type `localhost:3000/topics/1`.

- Go to your terminal, you should be able to interact with your `terminal`.

- type `params[:id]` in your terminal and it should return you the value of `1`.

- Run `exit` when you're done playing around. There'll be and error but that's okay, we've yet to build the view for it. Remove `binding.pry` when you're done.

- Now, let's build the `show` view by creating `show.html.erb` inside your `view/topics` folder.

- Paste following html:

  ```
  <h1><%= @topic.title %></h1>
  <p><%= @topic.description %></p>
  ```

- You should now be able to fetch specific topics from your database.
