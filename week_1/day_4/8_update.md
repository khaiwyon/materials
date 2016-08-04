## Update Action

- Update is the action to update a record, it requires an id parameter to be passed into to know which record is being updated.

- As usual, try adding `binding.pry` to your update method and make an edit. Play around and remove it (`binding.pry`) once you're done.

- Next, let's add the code to update:

  ```
  class TopicsController < ApplicationController

    def update
      @topic = Topic.find_by(id: params[:id])

      if @topic.update(topic_params)
        redirect_to topic_path(@topic)
      else
        redirect_to edit_topic_path(@topic)
      end
    end
  end
  ```

- It first searches for the topic and assigns it to `@topic`. Next, it will update the topic by calling the `update` method, passing in `topic_params`

- If it is successfully, it will redirect to the `show` page, otherwise it will redirect you back to the `edit` page is an error happens.
