# Add a Dash of AJAX
- Whenever you work with AJAX, its a good idea to start with the non-AJAX version of the application and then gradually introduce AJAX features
- Going to start by moving the cart from its own page and put it in the sidebar

## Moving the cart
- Currenty the cart is rendered by the `show` action in the CartController and corresponding `html.erb` template
- We want to put that rendering into the sidebar, meaning it will no longer be in its own page

### Rails partials
- Think of Rails partial templates as being a method for views
- A partial is simply a chunk of code of a view in its own separate file
- You can invoke (aka render) a partial from another template or from a controller, and the partial will render itself and return the results of that rendering
- As with methods, you can pass params to a partial, so the same partial can render different results

## Broadcasting Updates with Action Cable (p. 169)
Action Cable provies both a client-side JS framework and a server-side Ruby framework that together seamlessly integrate the websocket protocol into the rest of our app
- So far, user's browsers have requested info from the Rails app
- Its also possible to send info from Rails app to users browsers without a direct request through Web Sockets
- Enables features like real-time updates

Three step process:
1. Create a channel
2. Broadcast some data
3. Receive the data

Rails has a generator to set this up
`rails generate channel products`

1. Create a channel
- go to app/channels/products_channel.rb and uncomment the contents of the `subscribed` method

2. Broadcast some data
- in our case, we want to broadcast the whole product catalog anytime a product is updated, so we need to tap into the product controller's update method
- broadcast with: ActionCable.server.broadcast('products', html: render_to_string('store/index', layout: false))
  - we use `render_to_string` here and also don't render the layout
- broadcast messages typically consist of Ruby hashes, which are converted to JSON to go across the wire and end up as JS objects (we use `html` as the key in this case)

3. Receive the data on the client
- subscribing to the channel and defining what will be done with data is received
- in app/javascript/channels/products_channel.js, do some work in the `received(data)` method when we receive data
- in this case, we want to grab the `main.store` element using `querySelector`, and replace the inner html with `data.html` passed in from our controller
