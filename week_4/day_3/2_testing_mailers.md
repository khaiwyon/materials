# Day 3

## Password Resets Controller

- Create a test for your password reset controller.

- There is one extra check you have to make to see if your email was fired. In your unit testing the `create` action:
  - You should expect `ActionMailer::Base.deliveries.count` to eql to 1 (+1 from the email being fired)

- You should be a master by now ;)

## Testing your mailer

- Now that we've tested all our controllers. It's time to test our mailer associated with our password reset controller.

- `password_resets_mailer.rb`

```
class PasswordResetsMailer < ApplicationMailer

  def password_reset_mail(user)
    @user = user
    @url = "#{ENV.fetch('SERVER_URL')}/password_resets/#{@user.password_reset_token}/edit"
    mail(to: @user.email, subject: "Your password has been reset")
  end
end
```

- This is fairly simple and straightforward:

```
require 'rails_helper'

describe PasswordResetsMailer do
  before(:all) do
    @user = <create-your-user>
  end

  describe "should send email" do
    it "should send email with link to reset password" do

      @user.update(password_reset_token: "resettoken", password_reset_at: DateTime.now)
      mail = PasswordResetsMailer.password_reset_mail(@user)

      expect(mail.to[0]).to eql(@user.email)
      expect(mail.body.include?("#{ENV.fetch('SERVER_URL')}/password_resets/resettoken/edit")).to eql(true)
    end
  end
end
```

- Here, we simply mock a password reset token and generate the email by calling `PasswordResetsMailer` class before assigning it to `mail`

- Then, we check the contents inside `mail` to make sure it matches what we need to test (the user's mail, the correct link for example).

- REMEBER TO USE `binding.pry` TO FIND EXPLORE.
