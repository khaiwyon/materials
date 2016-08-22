# Day 3

## Model Tests

- Models are another important part of the application that we need to test. Here, we mostly test validations and any other important model methods that we have built.

- Similar to controller tests, this is the basic set up for model tests:

```
require 'rails_helper'

RSpec.describe User, type: :model do
end
```

- For the sake of learning, we'll use `context` instead of `describe`. `context` is essentially just an alias of `describe`.

- The first test we're writing is for associations, we'll need to make sure that the proper association for our user model is working. From our user model, we have:
  ```
  has_many :topics
  has_many :posts
  has_many :comments
  has_many :votes
  ```

- Therefore, using [shoulda-matchers](http://matchers.shoulda.io/docs/v3.1.1/) - we'll add the appropriate tests.

  ```
  context "assocation" do
    it { should have_many(:posts) }
    it { should have_many(:comments) }
    it { should have_many(:votes) }
    it { should have_many(:topics) }
  end
  ```

- Next, we'll need to test validations:

  ```
  context "email validation" do
    it { should validate_presence_of(:email) }
    it { should validate_uniqueness_of(:email) }
  end

  context "username validation" do
    it { should validate_presence_of(:username) }
    it { should validate_uniqueness_of(:username) }
  end
  ```

- They're fairly straightforward, we're testing the presence and uniqueness of email and password. Note that all these methods came from `shoulda_matchers`. It is an extremely powerful and productive library of writing model tests. The link above covers all the available methods you can use.

- Finally, we'll need to test our `slug callback`. Our `user` model had this callback from `friendly_id`

  ```
  before_save :update_slug

  def update_slug
    if username
      self.slug = username.gsub(" ", "-")
    end
  end
  ```

- We'll need to test this method too.

```
  context "slug callback" do
    it "should set slug" do
      user = create(:user)

      expect(user.slug).to eql(user.username.gsub(" ", "-"))
    end

    it "should update slug" do
      user = create(:user)

      user.update(username: "updatedname")

      expect(user.slug).to eql("updatedname")
    end
  end
```

- What it does is that we've created a new `user`, and expects our username after replacing all the spaces with dashes (`gsub(" ", "-")`) is equal to the slug.

- Similary, updating the title to `updatedname` will update the slug as well.

### Challenge

- Now that you've built your user model spec, it's time create one for each of the remaining models.

- BE SURE TO TEST YOUR COMMENT MODEL `TOTAL_VOTES` INSTANCE METHOD. I'LL LEAVE THIS TO YOU.
