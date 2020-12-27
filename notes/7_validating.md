# 7: Validating
- Make sure errors in the data never get committed to the database

## numericality
validates :price, numericality: { greater_than_or_equal_to: 0.01 }

## Validating price and money
- Always validate that a value is greater than or equal to .01
- A value like 0.001 could be entered in a form, which passes >= 0 validation, but ultimately ends up as 0 in the database if the column is correctly configured
  - We dont want that

## validating: format, allow blank, with, and message
```ruby
  validates :image_url, allow_blank: true, format: {
    with: %r{\.(gif|jpg|png)\Z}i,
    message: 'must be a URL for GIF, JPG, or PNG image.'
  }
```
- allow_blank is set to true because we already have a presence validation and we don't want to bubble up multiple errors
- the format option matches a field against a regular expression
