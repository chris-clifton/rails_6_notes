# truncate()
<%= truncate(strip_tags(product.description), length: 80 )% >
- Limits the text/string to 80 characters
- strip_tags removes the HTML tags from the description

# rails dbconsole
- Gets you into the sql console and allows you to interact directly with the database

# sanitize()
- Allows us to safely add HTMl stylings to make text more interesting

# number_to_currency()
- does exactly what it says

# assert_select()
- Gives us the ability to test HTML and CSS
```ruby
assert_select 'nav.side_nav a', minimum: 4
assert_select 'main ul.catalog li', 3
assert_select 'h2', 'Programming Ruby 1.9'
assert_select '.price', /\$[,\d]+\.\d\d/
```

