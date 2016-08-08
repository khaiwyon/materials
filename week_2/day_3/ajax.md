# Day 3

## Going AJAX

- [Ajax](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started) is the acronym for "Asynchronous Javascript And XML". In simple terms,
AJAX's greatest appeal is the ability to make a server-side call without refreshing the page itself.

- Let's start by first adding `respond_to :js` at the top of your `comments_controller`

  ```
  class CommentsController < ApplicationController
    respond_to :js
    before_action :authenticate!, except: [:index]

    <--- your remaining code --->
  end
  ```

- [respond_to](http://apidock.com/rails/ActionController/MimeResponds/InstanceMethods/respond_to) is a rails controller
method that sets what sort of format the action should respond to on top of the default `http`.

- We're next going to remove 
