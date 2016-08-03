## Destroy Action

- The final action is the destroy action, which allows you to delete topics.

- Go ahead and add the delete link to your `show` page.

- There are some minor differences:

  ```
    <%= link_to "Delete", topic_path(@topic), method: :delete %>
  ```

- the `delete` method is explicitly passed along because by default, link methods are `get` (REFER to CRUD again)

- Next, add the following code into your `delete` action in your controller.

  ```
  class TopicsController < ApplicationController

    def destroy
      @topic = Topic.find_by(id: params[:id])
      if @topic.destroy
        redirect_to topics_path
      else
        redirect_to topic_path(@topic)
      end
    end
  end
  ```

- Similar to create and update, destroy requires an id to find the specific topic you'd want to delete. `destroy` is the method built by Rails to delete a topic.
