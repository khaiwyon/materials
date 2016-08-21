# Day 1

## First Controller Test

- Controller tests are simply unit tests that tests a specific controller to ensure that it is working accordingly. `Unit tests` are tests written by a programmer for a programmer - to ensure that a specific class or method is working accordingly. This is in contrast to a `feature test`, in which tests are written to make sure that the correct `response or interaction` is performed by the software to the user. You can read more about it [here](http://stackoverflow.com/questions/2741832/unit-tests-vs-functional-tests)

- Let's start by writing the first test for our landing controller.

- Let's add a new file called `landing_controller_spec.rb` inside your `spec/controllers` folder.

- Read more about [rspec](http://rspec.info/documentation/) here. Referring to [this](http://www.rubydoc.info/gems/rspec-rails/frames#Controller_Specs).

- Let's layout the basic structure of a controller test.

```
  require 'rails_helper'

  RSpec.describe LandingController, type: :controller do
  end
```

- This is the basic structure, all test files should `require 'rails_helper'`

- The second is to initialize `RSpec` by determining which controller we are testing (in this case it's the `LandingController`), `type: :controller` describes itself, we're letting RSpec know
that we are testing a controller. Finally we'll set up a block to pass in to `RSpec`

- Next, we'll need to describe the goal of each test by using the method `describe` and `it`. For example:

```
require 'rails_helper'

RSpec.describe LandingController, type: :controller do

  describe "index" do
    it "should render index" do
    end
  end
end
```

- `describe` is to set a category of the test; `it` is to explain the scenario of that particular test. Note that each test can have multiple `describe` inside it, and each `describe` can
have multiple `it` inside them.

- For this test, we're going to make sure that our landing controller renders the proper index page.

- Add the following into your `it` block:
  ```
    get :index
    expect(subject).to render_template(:index)
  ```

- More theory: `get` is the type of `action` (remember CRUD) that calls the specific controller action (in this case it's `index` as defined inside your `LandingController`)

- `get :index` means we are making a `GET` request to `landing#index`.

- The next line is the `expect`. `expect` is used when we expect the appropriate response of the controller action. `subject` is the value rendered from the controller. `render_template(:index)` is a rspec [matcher] to check that calling `landing#index` actually renders `landing/index.html.erb`.

- The RSpec is extremely huge and extensive, I will not be able to cover everything in 1 week - but will provide you enough to write adequate tests for your application. You will need to refer to the [documentation](http://rspec.info/documentation/) often and google.

- FINAL example,

```
  require 'rails_helper'

  RSpec.describe LandingController, type: :controller do

    describe "index" do
      it "should render index" do

        get :index
        expect(subject).to render_template(:index)
      end
    end
  end
```

- now let's go ahead and type `rspec` in your terminal to run it. You can also test a specific controller by typing `rspec <your-target-file>`, in this case: it's `rspec spec/controllers/landing_controller_spec.rb`
