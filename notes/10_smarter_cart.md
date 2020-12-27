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
