# 13: Entering Additional Payment Details
Chapter focuses:
- using webpacker to manage app-like javascript
- setting up dev environment that includes webpack
- using React to build a dynamic web form
- using Capybara and ChromeDriver to test javascript powered features

## Adding fields dynamically to a form
We need a dynamic form that changes what fields are shown based on what pay type the user has selected

## Webpacker
- Webpacker is a gem thats part of Rails and sets up Webpack inside our app
- Webpack is a tool to manage javascript files

React is a JavaScript view library designed to quickly create dynamic user interfaces.  We'll use it to create a dynamic payment method details form, and Webpacker will ensure that the configs are setup for this

## Installing React with Webpacker
rails webpacker:install:react

## Updating our Development Environment for Webpack
Webpacker includes a helper method called `javascript_pack_tag()` that takes as an argument the name of the file in `app/javascript/packs` whose JavaScript should be included on the page

The reason Rails doesn't include all JavaScript all the time is that you might not want that to happen for performance reasons.

Since we're building a React component for the payment process, we probably dont want to force users to download it every time.

Webpacker allows us to have any number of these separately managed packs and we can include them whenever and wherever we like.
