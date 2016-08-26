# Shopping cart and cookies

- Implementing a shopping cart with Rails is fairly simple, each item we're displaying in the store has an ajax link that saves the items id into a cookies table.

- Referring to this [documentation](http://api.rubyonrails.org/classes/ActionDispatch/Cookies.html). We can store the item id in the form of a hash.

- cookies.permanent.signed.encrypted[:cart] = { item_1_id: quantity, item_2_id: quantity, item_3_id: quantity }

- The concept is simple:
  - If a user adds a new item into the cart, it looks for the id in the cookies hash and adds the amount to it or creates a new entry in the hash with the quantity.
  - If a user removes an item, it either deletes the entry if the quantity reaches 0, otherwise it just deducts the amount with the total stored in the hash.

- Note to use `signed` and `encrypted`, we want to keep our users from tampering with it or reading it.
