# Day 3

## Challenge

- Let's now test your user role, go to `rails c` and update one user in your record to by admin.

  ```
    rails c

    <-- Inside terminal -->
    user = # Pick one user
    user.update(role: :admin)
  ```

- Building from your lesson with authorization. Do the same thing for users, posts and comments.

- The requirement is that admin and moderators can edit and delete and comment or post. But users can only edit or delete their own post or comment - not others.

- Here's one example to get you started.

  ```
  class CommentPolicy < ApplicationPolicy

    def edit?
      user.present? && record.user == user || user_has_power?
    end

    private

    def user_has_power?
      user.admin? || user.moderator?
    end
  end
  ```

- The first checks for whether the user is present, the second checks to see if the record belongs to the user or if the user is an admin or moderator.
