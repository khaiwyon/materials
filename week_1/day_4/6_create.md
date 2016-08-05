
## Create Action

- Remove `binding.pry` when you're done, now let's save the data sent from your form as a new topic in your database.

- At the bottom of your code before `end`, add the following code:

  ```
  class TopicsController < ApplicationController

  <-- Your Code -->

  private

    def topic_params
      params.require(:topic).permit(:title, :description)
    end
  end
  ```

- The code you added is refered to as [Strong Parameters](http://edgeapi.rubyonrails.org/classes/ActionController/StrongParameters.html)

- From the documentation:

  ```
  Strong Parameters provide an interface for protecting attributes from end-user assignment.
  This makes Action Controller parameters forbidden to be used in Active Model mass assignment until they have been whitelisted.
  ```

- Now add the following code into your `create` method:

  ```
  class TopicsController < ApplicationController

    def create
      @topic = Topic.new(topic_params)

      if @topic.save
        redirect_to topics_path
      else
        redirect_to new_topic_path
      end
    end
  end
  ```

- `Topic.new` with the argument `topic_params` passed into it essentially means:

  ```
  Topic.new(title: <your-title-string>, description: <your-description-string>)
  ```

- You should remember `if .. else` from your ruby lessons. It checks to see if `@topic.save` was saved, then it run `redirects_to` the appropriate paths depending on
whether you successfully saved the record or not.

- Now give it a try, create a new record, you should be redirected to the index page and your new record should be displayed now.
