# 10: A Smarter Cart
- Have the basics of a cart already, but have a lot left to do
- Associating a count with each product

## Error Handling
The Flash
- Rails has a convenient way of dealing with errors and error reporting: the flash
- Flash is a bucket (clser to a Hash) where you can store stuff as you process a request
- The contents of the flash are available to the next request in this session before being deleted automatically

Why cant we store flash messages in instance variables?
- After a redirect is sent by ur application to the browser, the browser sends a new request back to our application
- By the time we receive that second request, our application has moved on and all instance variables from previous requests are long gone
- Flash data is stored in the session to make it available between requests

Rescuing with `rescue_from`
- Clause intercepts exception raised by `Cart.find()`.  In the handler we do two things:
  1. Use the Rails logger to record the error (every controller has a logger attribute)
  2. Redirect to the catalog display by using the `redirect_to` method
    - The `:notice` parameter specifies a message to be stored in the flash as a notice

Why redirect rather than display the catalog here?
- If we redirect, the user's browser will end up displaying the store URL, rather than `http://.../cart/wibble`
- We expose less of the pplication this way and prevent the user from retriggering the error by clicking the Reload button

## product_path vs product_url
When to use `product_path` and when to use `product_url`

## product_url
- When you see `product_url` you'll get the full enchilada with protocol and domain name
- This is the thing to use when you're doing `redirect_to` because the HTTP spec requires a fully qualified URL when doing 302 Redirect
- Also need a full URL if you're redirecting from one domain to another

## product_path
- The rest of the time you can happily use `product_path`