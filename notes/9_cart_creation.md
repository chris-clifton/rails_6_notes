# 9: Cart Creation
- App needs to keep track of all the items added to the cart by the buyer
- We'll keep a cart in the database and store its unique identifier (cart.id) in the session
- Generate another scaffold for Cart

# set_cart()
- Create a concern in the controllers dir
- Create a module named CurrentCart with a single private method set_cart
- set_cart should look for a cart in the session, and if it cant find one, create one and then store it in the session

## line_item scaffold
- We have the cart scaffold going, and we know that the cart will be filled with line_items
- Scafford out line_items
rails g scaffold LineItem product:references cart:belongs_to 

## belongs_to
- Our line_item model belongs to a cart
- Good rule of thumb for belongs_to is that if a table has any foreign keys, the corresponding model should have a belongs_to for each
- Need to add a `has_many` to the Cart model

## Add to cart button
- cart#create
