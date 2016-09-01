# Shopping cart and cookies

- Implementing a shopping cart with Rails is fairly simple, each item we're displaying in the store has an ajax link that saves the items id into a cookies table.

- Referring to this [documentation](http://api.rubyonrails.org/classes/ActionDispatch/Cookies.html). We can store the item id in the form of a hash.

- cookies.permanent.signed.encrypted[:cart] = { item_1_id: quantity, item_2_id: quantity, item_3_id: quantity }

- The concept is simple:
  - If a user adds a new item into the cart, it looks for the id in the cookies hash and adds the amount to it or creates a new entry in the hash with the quantity.
  - If a user removes an item, it either deletes the entry if the quantity reaches 0, otherwise it just deducts the amount with the total stored in the hash.

- Note to use `signed` and `encrypted`, we want to keep our users from tampering with it or reading it.

- Extra Hint:
  - For some weird reason, the method `to_sym` was implemented for `Fixnum` in Ruby. So you will have to use `=>` to assign an `Integer` in a hash. E.G:
    - { item_id => 1, item_2_id => 3 }

- Finally, you need to use `JSON.generate` and `JSON.parse` read and serialize your hashes since cookies only accept this in string format. Reference from documentation here:
  ```
  # Cookie values are String based. Other data types need to be serialized.
  cookies[:lat_lon] = JSON.generate([47.68, -122.37])
  ```

## To serialize your hash

- cookies[:cart] = JSON.generate({ item_id => 1, item_2_id => 2})

## To read your serialized hash

- cart_items = JSON.parse(cookies[:cart])

- Find out how to read hashes [here](http://docs.ruby-lang.org/en/2.0.0/Hash.html)
