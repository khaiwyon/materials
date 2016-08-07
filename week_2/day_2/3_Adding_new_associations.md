# Day 3 Lessons

## Authenticating Users

- Now that we've set up users and authentication, it's finally time to tie it all up with your 3 existing models.

- First, let's add the associations for your users, it should have `has_many` with the `topics`, `posts`, and `comments`

- Next, add the proper `migrations` for your 3 models - adding `user_id` as an `integer` into their tables.

- Finally, you can make the following changes to your topics controller `create` method.

  ```
  def create
    @topic = current_user.topics.build(topic_params)

    if @topic.save
      redirect_to topics_path
    else
      redirect_to new_topic_path
    end
  end
  ```

- FURTHER READING:

[building associations](http://guides.rubyonrails.org/association_basics.html#detailed-association-reference)
