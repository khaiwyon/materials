# Payment with Billplz

- Create an account with [Billplz](https://www.billplz.com/).
  - NOTE: You will need to enter your bank details to complete the signup.
  - NOTE: Billplz may charge you a minimum RM1 for verification.
- Install the [`httparty gem`](https://github.com/jnunemaker/httparty).
- Once verified, create a new collection for your bills. In Billplz, a collection `has_many` bills.
- **Read the example payment flow [here](https://www.billplz.com/api#api-flow).**
- Create a new `billplz.rb` model file and define a method to create new bills.
- Create `webhooks` to receive server side request from Billplz.
- You can refer to the full documentation [here](https://www.billplz.com/api#v3).
