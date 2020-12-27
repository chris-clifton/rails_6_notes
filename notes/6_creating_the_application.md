# 6: Creating the Application
- Going to go ahead and just get started creatig an initial web interface that lets us maintain product information
- run rails new!

## Setting up the database
- sticking with sqlite 3 so nothing to do here

## Generating the Scaffold

## Seeding database
- Edit seeds.rb
  - Use `.create!` to make some Products
- rails db:seed

## Controller based-css
- For the products controller, go to `products.scss`
  - create a `.products` class and fill out some CSS
- Go to application.html.erb
  - Wrap the `yield` in  `<main class='<%= controller.controller_name %>'></main>`
