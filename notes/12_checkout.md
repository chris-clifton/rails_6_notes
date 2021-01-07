# 12: Checkout
Chapter focuses:
- linking tables with foreign keys
- using `belongs_to`, `has_many`, and `:through`
- creating forms based on models (`form_with`)
- linking forms, models, and views
- generating a feed using `atom_helper` on model objects

## Capturing an Order
Orders are a set of line items, along with details of the purchase transaction.  Our cart already contains line_items so alll we need to do is add an order_id column to the line_items table and create an `orders` table based on the Initial Guess at application data diagram

Create the `order` model and update the `line_items` table
`rails generate scaffold Order name address:text email pay_type:integer`
- Note that name and email default to the string data type

`rails generate migration add_order_to_line_item order:references`
- modify this migration so that `cart_id` can be null in records

### Adding a foreign key in Rails 6
rails generate migration add_foreign_key_name_to_table_name foreign_key_name:references

### enums
- Add an enum declaration for pay_type to the order model
```ruby
enum pay_type: {
  'Check' => 0,
  'Credit card' => 1,
  'Purchase order' => 2
}
```

## Creating the order capture form
- First we need a checkout button on the shopping cart

## form_with
Does two big things
1. Sets up an HTML form that knows about Rails routes and models
2. Creates the form fields themselves

## Atom feeds
-  Rails' `atom_feed` helper provides sensible defaults for an out of the box feed
