# Payment with Braintree

- You will need an account from [braintree](https://www.braintreepayments.com/sandbox) to get started.

- Create a new set of API keys that you can use to communicate with Braintree's server. You can find them under `account -> my user -> view authorizations`.

- You can create a new `braintree.rb` initializer file with the following configuration:

```
Braintree::Configuration.environment = :sandbox
Braintree::Configuration.merchant_id = ENV['MERCHANT_ID']
Braintree::Configuration.public_key = ENV['PUBLIC_KEY']
Braintree::Configuration.private_key = ENV['PRIVATE_KEY']
```

- You can now set up server side using the following [link](https://developers.braintreepayments.com/start/hello-server/ruby)

- As for your payment form, [here](https://developers.braintreepayments.com/guides/drop-in/javascript/v2)

HINTS:

- You need to find a way to pass `@token = Braintree::ClientToken.generate` to your js file. Use your controllers and jQuery to work this out.
