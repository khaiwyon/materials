# Day 4

## Feature Tests

- We've covered model tests and feature tests, we'll now cover a few simple feature tests. I'll start by showing registering and then you can work out the rest.

- Setting up a feature test:

```
require "rails_helper"

RSpec.feature "User Management", type: :feature, js: true  do
end
```

- Instead of describe, features are about `scenarios`, defined as such:

```
require "rails_helper"

RSpec.feature "User Management", type: :feature, js: true  do
  scenario "User registers" do
  end
end
```

- I'll know fill the scenario up and explain play by play.

```
require "rails_helper"

RSpec.feature "User Management", type: :feature do
  before(:all) do
    @user = create(:user)
  end

  scenario "User registers" do

    visit new_user_path
    fill_in 'Username', with: 'ironman'
    fill_in 'Email', with: 'ironman@email.com'
    fill_in 'Password', with: 'password'

    click_button('Sign me up!')

    user = User.find_by(email: "ironman@email.com")

    expect(User.count).to eql(2)
    expect(user).to be_present
    expect(user.email).to eql("ironman@email.com")
    expect(user.username).to eql("ironman")
    expect(find('.flash-messages .message').text).to eql("You're registered, welcome!")
    expect(page).to have_current_path(root_path)
  end
end
```

- `Feature tests` are very straightforward, the methods are straightfoward and simple to grasp: click this

- `visit` is to visit a specific url, in this case I've jumped straight to `new_user_path`, or the `registration page`.

- `fill_in` is to look for a field or textbox (in this case I've searched for the label `Username`) and fill it up.

- `click_button` is to click a button with the content ('Sign me up!')

- `expect(find...)` => `find` is also a `capybara` method for making sure a certain element or content is available once a user performs a specific action.

- Note that almost all of capybara's methods can find either the `css (id or class)`, `content`, and even `labels`.

- I've included a few cheat sheets to easily help you get started:
  - [Documentation](https://github.com/jnicklas/capybara)
  - [Cheat Sheet 1](http://cheatrags.com/capybara)
  - [Cheat Sheet 2](https://thoughtbot.com/upcase/test-driven-rails-resources/capybara.pdf)
  - [Cheat Sheet 3](https://gist.github.com/zhengjia/428105)

## Challenge

- Complete the following scenarios:
  - A user logging in
  - A user logging out
  - A user editing and updating his/her profile
  - A user navigating through various pages (topics, posts, comments index, password forgot page, about page, etc)

## Testing ActionCable

- Note that with action cable, the test server currently does not work, so we'll be using the development server for this (you will to start your rails server for this):

```
require "rails_helper"

RSpec.feature "User adds new comment and deletes it", type: :feature, js: true do

  scenario "User adds, edits, upvotes, downvotes, and delete comment" do
    visit("http://localhost:3000")

    click_link("Login")

    fill_in 'Email', with: "eldrethlim@gmail.com"
    fill_in 'Password', with: "newpassword"
    click_button("Log me in!")

    click_link("topics-index")
    click_link("Programming")
    click_link("Rails")

    fill_in "Body", with: "This is a new comment"
    click_button("Comment")
    expect(page).to have_content("This is a new comment")

    click_button("close-flash")

    click_link("Edit Comment")
    edit_form_body = find("#comment-edit-form #comment_text_field").set("This is an edited comment")
    click_button("Update")
    expect(page).to have_content("This is an edited comment")

    click_button("close-flash")

    click_link("Upvote")
    expect(find(".voting-score")).to have_content("1")

    click_button("close-flash")

    click_link("Downvote")
    expect(find(".voting-score")).to have_content("-1")

    click_button("close-flash")

    click_link("Delete Comment")
    # accepts alert
    page.driver.browser.switch_to.alert.accept
    expect(page).to_not have_content("This is an edited comment")
  end
end
```

- Try and understand the source codes and build it play by play:
  - NOTE THAT THE STARTING POINT IS YOU VISITING `http://localhost:3000`

- NOTE: use `js: true` if you need to test certain `js` functions in your app. For example, `modals`.
