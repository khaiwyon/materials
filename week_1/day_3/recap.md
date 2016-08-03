# Day 3 Lessons

## Recap

- Let's recap what we've learnt for the past two days in the form of an exercise.

- In this exercise, you are required to build an about page for your discussion board.

- Firstly, create a new controller called `static_pages_controller.rb` in your `app/controllers folder`

- Add the following lines of code:

  ```
  class StaticPagesController < ApplicationController

    def about
    end
  end
  ```

- Next, create a new folder under `app/views` called  `static_pages`

- Create `about.html.erb` inside your newly created `static_pages` folder

- Finally, let's set up the routes by going to your `config/routes.rb`

- Add the following below `root to: 'landing#index' `

  ```
  get :about, to: 'static_pages#about'
  ```

- For more information about routing, refer to the [documentation](http://guides.rubyonrails.org/routing.html)

- Finally, create `about.scss` inside `app/assets/stylesheets/views` folder.

- This is where you'll add styling for the about page.

- Finally, before you get started, head over to your `landing.scss` file. Wrap the entire code into a single class
called `.landing`. Example:

  ```
  .landing {
    // Your landing page stylesheets here
  }
  ```

- Do the same for your `landing.html.erb`

  ```
  <div class="landing">
    // Your landing page html in here
  </div>
  ```

- Repeat the same for your header and about pages

- This is to scope your stylesheets into a specific page to prevent css 'spillage'
to your other pages.

- Finally, let's add a link for your about page to the header. Head to your `_header.html.erb` file
and remove the following code:

  ```
  <li><a href="#">Topics<span class="sr-only">(current)</span></a></li>
  <li><a href="#">Log In<span class="sr-only">(current)</span></a></li>
  ```

- Replace it with:

  ```
  <li><%= link_to "About", about_path %></li>
  ```

- Now remove the following code:
  ```
  <a class="navbar-brand" href="#">MagicForums</a>
  ```

- Replace it with:

  ```
  <%= link_to "MagicForums", root_path, class: "navbar-brand" %>
  ```

- Find out more about `link_to` [here](http://apidock.com/rails/v3.2.8/ActionView/Helpers/UrlHelper/link_to)

#### Criteria for your about page:

- It should contain a full page photo, with a text in the center detailing what
your discussion board is all about.

- The people (namely you) behind this discussion board. Add a short but sweet description about yourself.

- Use icons to tell the story. Add a few more that links specifically to your social networks.

- HINT: use `link_to ... do` to add an icon to your links. Refer to the documenation for more ideas.
