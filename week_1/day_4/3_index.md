## Index Action

- The index action is used to list all of records of a model.

- Let's create a new folder inside `views` called `topics`, and a new `html` page called `index.html.erb`

- Add the following code in it:

  ```
  <h1>Topics</h1>

  <% @topics.each do |topic| %>
    <div class="topic">
      <h3><%= topic.title %></h3>
      <h5><%= topic.description %></h5>
      <%= link_to topic.title, topic_path(topic) %>
    </div>
  <% end %>
  ```

- Next, add the following line inside your `index` method in your `topics_controller.rb`

  ```
  @topics = Topic.all
  ```

- Referring to the [docs](http://guides.rubyonrails.org/active_record_querying.html#retrieving-multiple-objects-in-batches),
`.all` is a method call that fetches all available records of a given model, which is in this case the `Topic` model.

- We've taken all records and passed it into an [instance variable](http://ruby-doc.org/core-2.1.2/Object.html#method-i-instance_variables), marked by the symbol `@`

- Now head to `localhost:3000/topics` and observe your work. You should see a list of topics that you've created from your lessons yesterday.

- The link should still be broken so let's fix that in a while.

- For now, try adding new topics in your `rails console` and refresh your page.

FURTHER READING

- [Each method](http://ruby-doc.org/core-2.2.0/Array.html#method-i-each)
- [Embedded Ruby](https://en.wikipedia.org/wiki/ERuby)
- [Ordering your records](http://guides.rubyonrails.org/active_record_querying.html#ordering)
