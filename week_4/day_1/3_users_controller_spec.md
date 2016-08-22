# Day 1

## Users Controller Spec

```
require 'rails_helper'

RSpec.describe UsersController, type: :controller do
end
```

- Set up your controller as usual. We now have 4 controller actions to test - `new, create, edit, and update`. Each will have a different scenario as well. For example, we will have to test
the authorization of edit and update, as well as authentication (i.e - they should be redirected to the landing page if they're not logged in)

### Setting up a the proper model objects

- We're now going to set up 2 user records for this test that we will use for this test suite. To do this, we're going to use a `before(:all)` block. This makes any record created here available to all test cases later so we do not have to recreate them again and again for every test case.

- Add the following code:
  ```
    before(:all) do
      @user = User.create(<define-your-user-params-here>)
      @unauthorized_user = User.create(<define-your-user-params-here>)
    end
  ```

- Now we will have `@user` and `@unauthorized_user` available to this test suite.

### New

- We'll start with the easiest - new (NOTE: IF YOU DID THE MODAL CHALLENGE LAST FRIDAY - YOU SHOULD SKIP THIS PART AS YOUR FORM HAS BEEN MOVED TO A MODAL)

```
describe "render new" do
  it "should render new" do

    get :new
    expect(subject).to render_template(:new)
    expect(assigns[:user]).to be_present
  end
end
```

- Fairly straightforward, with something new at the last line. `expect(assigns[:user]).to be_present` essentially checks to make sure that `@user = User.new` from your `users#new` is working.

- Play around by adding binding.pry and typing `assigns` in your terminal to look around what actually is in it.

### Create

- Next, we'll need to test `create`.

```
describe "create user" do
  it "should create new user" do

    params = { user: { email: "user@gmail.com", username: "thor", password: "password" } }
    post :create, params: params

    user = User.find_by(email: "user@gmail.com")

    expect(User.count).to eql(3)
    expect(user.email).to eql("user@gmail.com")
    expect(user.username).to eql("thor")
    expect(flash[:success]).to eql("<your-flash-message>")
  end
end
```

- I'll explain line by line:

- `params = { user: { email: "user@gmail.com", username: "thor", password: "password" } }` sets the params that we are planning to pass to the controller. Note that because of `strong_params`:- we need to wrap the appropriate data inside the name of the permitted parameter. In this case it's `user`, so we'll need to wrap `email`, `username`, and `password` inside it.

- `post` is the type of action we'll use to send to `:create` - which stands for `users#create` in your `routes.rb`

- Once the action is fired, we'll retrieve the `user` object by finding it with `find_by` (You should still remember this)

- We'll need set a series of `expectations` that the user is properly registered into the database.

```
  expect(User.count).to eql(3)
  expect(user.email).to eql("user@gmail.com")
  expect(user.username).to eql("thor")
  expect(flash[:success]).to eql("<your-flash-message>")
```

- The first checks that the number of records in your `User` model has increased by 1 to a total of 3 (remember the two initially created in `before(:all)`).

- The second checks that the `user` record we fetched is the same as the one we sent in via `params`.

- The third checks the same for `username`

- Finally, we expect the flash message that is displayed when the user successfully registers to be rendered.

### Edit

```
describe "edit user" do

  it "should redirect if not logged in" do

    params = { id: @user.id }
    get :edit, params: params

    expect(subject).to redirect_to(root_path)
    expect(flash[:danger]).to eql("<your-flash-message>")
  end

  it "should redirect if user unauthorized" do

    params = { id: @user.id }
    get :edit, params: params, session: { id: @unauthorized_user.id }

    expect(subject).to redirect_to(root_path)
    expect(flash[:danger]).to eql("<your-flash-message>")
  end

  it "should render edit" do

    params = { id: @user.id }
    get :edit, params: params, session: { id: @user.id }

    current_user = subject.send(:current_user)
    expect(subject).to render_template(:edit)
    expect(current_user).to be_present
  end
end
```

- We now have 3 scenarios for `edit user`. The first one checks to make sure that an unauthenticated user is not allowed to edit it, the second checks to see to make sure an unauthorized user cannot update another user's details, and the final one makes sure the proper user can.

- NOTE: when making a controller action call like this (for the second and third scenario) `get :edit, params: params, session: { id: @user.id }`:
  - the first parameter is the controller action (`:edit` in this case).
  - the second argument accepts the `params`
  - the third is to pass in any `session`. In this case, we're passing in the user id to show that this user is logged in (see session controller `session[:id] = @user.id`)

- `subject.send(:current_user)` is used when we need to retrieve a method from a parent class. In this case, we needed to match the `current_user` method from the application controller.

### Update

- Finally, we have update:

```
describe "update user" do

  it "should redirect if not logged in" do
    params = { id: @user.id, user: { email: "new@email.com", username: "newusername" } }
    patch :update, params: params

    expect(subject).to redirect_to(root_path)
    expect(flash[:danger]).to eql("<your-flash-message>")
  end

  it "should redirect if user unauthorized" do
    params = { id: @user.id, user: { email: "new@email.com", username: "newusername" } }
    patch :update, params: params, session: { id: @unauthorized_user.id }

    expect(subject).to redirect_to(root_path)
    expect(flash[:danger]).to eql("<your-flash-message>")
  end

  it "should update user" do

    params = { id: @user.id, user: { email: "new@email.com", username: "newusername", password: "newpassword" } }
    patch :update, params: params, session: { id: @user.id }

    @user.reload
    current_user = subject.send(:current_user).reload

    expect(current_user.email).to eql("new@email.com")
    expect(current_user.username).to eql("newusername")
    expect(current_user.authenticate("newpassword")).to eql(@user)
  end
end
```

- This is fairly similar to `edit` and `create`; but note the final scenario. Here we needed to `reload` both `@user` and `current_user` because the data inside the database has been updated to the new values but the `objects - @user and current_user` are still cached with the older one. Hence, we needed to run `reload` on the both of them to make sure they reflect the latest data.

- Take a moment to fully understand how rspec works for this controller. REMEMBER THAT YOU CAN USE `BINDING.PRY` AT ANY POINT AND PLAY AROUND TO SEE WHAT IS ACTUALLY GOING ON - THIS APPLIES TO THE CONTROLLER ACTION YOU ARE CALLING

- TRY OUT THE VARIOUS COMMANDS AVAILABLE - e.g `SUBJECT`, `RESPONSE`, `FLASH` to see the inner workings of rspec.

### Challenge

- Once you have finished, using what you've learnt here:
  - Complete the controller tests for `Topics`, `Posts`, and `Comments`
  - Make sure to cover at least the following scenarios:
    - What happens if the user is not logged in
    - What happens if the user is not authorized
    - What happens if the user has all the right credentials
  - HINT: If you are creating an ajax action, you will need to pass in `xhr: true` to your action as well:
    - For example: `post :create, xhr: true`
