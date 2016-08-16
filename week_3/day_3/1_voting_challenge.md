# Day 5

## The Voting Challenge

- We're now going to implement a voting feature for comments.

- Two buttons will be available to users, a `+` and a `-` to mark if they like or dislike the comment or post.

- When a user clicks to vote, it will search for the votes table for an existing vote for that post or comment, if it exists, it will update the value of vote with `+1` or `-1`, otherwise it will create a new vote.

- Voting score is calculated by the counting the number of +1 and -1 votes.

### HINTS TO GET YOU STARTED:

- Create a new model called `Vote`. It has 3 attributes - `value`, `user_id`, `comment_id`

- The associations are:

  ```
    class Vote < ApplicationRecord
      belongs_to :user
      belongs_to :comments
    end

    class Comment < ApplicationRecord
      has_many :votes
    end

    class User < ApplicationRecord
      has_many :votes
    end
  ```

- Build a votes controller. Create 2 custom methods called `upvote` and `downvote`. Create 2 new `POST` routes called `/upvote` and `/downvote`. I'll help you with one:

  ```
    post :upvote, to: 'votes#upvote'
  ```

- Both method works like this:
  - It searches for the votes table for the matching user and comment.
  - If a record is found, it updates that record with the `value` of `+1` or `-1`
  - If a record is not found, it creates a new record with the `value` of `+1` or `-1`.

- HINT: You can try using [this](http://apidock.com/rails/v4.0.2/ActiveRecord/Relation/find_or_create_by) method.

- Create a new method called `votes` inside your `comment` model.

- `votes` method makes 2 queries: it counts the number of `+1` votes and `-1` votes and tallies them. Check the `IMPORTANT` methods to use.

- EXAMPLE:
  ```
    class Comment < ApplicationRecord
      <-- your-code-here -->
      def total_votes
        <-- your-challenge-here -->
      end
    end
  ```

- This will allow you to call a new attribute - `total_votes`. Try it in your console:

  ```
    c = Comment.first
    c.total_votes
  ```

- HOW TO GET YOUR TOTAL VOTES:
  - fetch the values of all your votes associated with the comment
  - tally the total of values
  - Hint: use [pluck](http://apidock.com/rails/ActiveRecord/Calculations/pluck) and [sum](http://apidock.com/rails/Enumerable/sum)

- If you're feeling very ambitious, add these features:
  - If a user has voted on this post or comment, it will highlight the button with another color.
  - The ability to un-vote. Currently if a user has vote, they can only change it with a `-1` or `thumbs down` vote or vice versa.

- Tie this in with ActionCable and AJAX, if a user votes it would reflect on the views for others as well.
