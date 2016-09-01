# Building a shopping cart

- Building a shopping cart is all about manipulating your cookies.

```
class CartsController < ApplicationController

  def show
    items = JSON.parse(cookies[:cart])

    @items = []
    items.each do |k,v|
      # your k is your item id
      # your v is your quantity
      # you can also rename k to item_id, and v to quantity for clarity
      # search for your item
      item = Item.find_by(id: k)

      # creates a new method and assigns the quantity to it
      item.define_singleton_method(:quantity) do
        v
      end
      @items << item
    end
  end

  def add_item
    if !cookies[:cart] #checks if an existing cart exists
      items = { params[:id] => params[:quantity] }
    else
      items = JSON.parse(cookies[:cart])
      items[params[:id]] = params[:quantity]
    end

    cookies[:cart] = JSON.generate(items)
  end

  def update_item
  end

  def remove_item
    // Use delete
  end

  def show
  end
end

- Your routes:

# get :cart, to: "carts#show"
# post :add_item, to: "carts#add_item"
# post :remove_item, to: "carts#remove_item"

- In your views, create a simple form that sends 2 parameters to your carts controller, the item id and the item quantity.

- Use hash [delete](https://ruby-doc.org/core-1.9.3/Hash.html#method-i-delete) to delete key / value pairs from your hash.

- Be sure to encrypt and sign your cookies (refer to `shopping cart and cookies`)

- Read about `define_singleton_method` [here](http://apidock.com/ruby/Object/define_singleton_method)
