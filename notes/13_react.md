# 13: React
- What is a .jsx file?
- What is the odd syntax inside `app/javascript/packs/hello_react.jsx`?

## What is React?
React is a view library for JavaScript.

Like .erb files we've been using, React dynamically renders HTML.  Unlike ERB, React does this in the browser and is optimized to do it fast.  Because the selected pay type will only affect a small part of our page, it will be much better UX to have React rerender that part of our page than have the server rerender the entire thing.

React is more than just a library with some handy functions we can call.

Its a mini-framework that includes extensions to JS to make our work easier- once we understand how to use those extensions.  When we do, our job of creating a dynamic payment details form will result in easy-to-understand code thats also easy to manage, thanks to Webpacker

## Components
Components are the core concept in React.

A component is a view, backed by some sort of state.  When the state changes, the view rerenders.

The view can behave differently depending on the current state inside the component.

## JSX
We could do this with React's JavaScript API, but the code would be verbose, difficult to read, and hard to maintain.  Using React's extensions to JS, which is JSX, this can be made a little easier.

JSX allows you to intermix HTML-like markup and JavaScript code in one file.

React provides a compiler from JSX to JavaScript and Webpack can use that compiler as part of its build process

# Creating a React-powered drop-down
First, to get a sense of how to work with React and Webpack, replace the existing pay type drop-down thats being rendered by Rails with one thats rendered by React

This can be done in three steps:
1. Create a new pack called `pay_type` that will be the root of our implementation
2. Create the `PayTypeSelector` component that we'll use to replace the existing pay type selector drop down
3. Bring the component into our checkout view using `javascript_pack_tag()` and a piece of markup that React can hook into to render the component

## 1. Creating a new pack
Packs go in `app/javascript/packs`
- Create `app/javascript/packs/pay_type.jsx`

This code is not a React component- its just a few lines of code to bootstrap our React component and get it onto our page
```javascript
import React            from 'react'            // #1
import ReactDOM         from 'react-dom'        // #2
import PayTypeSelector  from 'PayTypeSelector'  // #3

document.addEventListener('turbolinks:load', function() {       // #4
  let element = document.getElementById('pay-type-component');  // #5
  ReactDOM.render(<PayTypeSelector />, element);                // #6
});
```
1. This is how we get access to the main React library.  `import` is like `require`: it allows us to access code located in other files
  - Although its part of the JS standard, browsers dont support it
  - Webpack provides an implementation for us when it compiles our code
  - When Webpack processes this line, it'll try to find a file named `react.js` in one of the paths its configured to search
2. This brings in the ReactDOM object, which has the `render()` function we need to bootstrap our React component
3. Here, we're importing `PayTypeSelector`, which is the component we'll make
4. Turbolinks manages the page-loading events for us and instead of regular DOMContentLoaded events (etc.), it fires the `turbolinks:load` event
  - Using `turbolinks:load` ensures that React is setup every time the page is rendered
5. Pretty standard here, just getting an element to work with
6. `ReactDOM.render()`'s job is to replace `element` with the React component `PayTypeSelector`
  - In a JSX file, the way to do that is via this odd HTML-like value `<PayTypeSelector />`
    - Eventually Webpack compiles this down into regular JS that works in the browser
    - This works because we used `PayTypeSelector` in the import line (#3)

## 2. Creating the `PayTypeSelector` component
We talked about what `import` does but now we need to know more about how it does it

When Webpack is compiling our files into a bundle our browser can understand, its configured with certain paths it will use to locate files we ask to import.
The first path is `node_modules`, which is where Yarn downloaded all our third-party javascript libraries, including React. It has a bunch of stuff, but `react` and `react-dom` are in there.

Our code doesn't go in `node_modules` though, and goes in `app/javascript`. Webpacker has configured Webpack to also look there for files to import.

Webpack isnt just looking for files like `app/javascript/PayTypeSelector.jsx`.  Rails and Webpack both want us to organize our JavaScript into multiple files, so when we ask to import `PayTypeSelector`, Webpack will load the file `app/javascript/PayTypeSelector/index.jsx`
- May seem odd but this is how third party JS is bundled
- Also allows us to organize files needed by `PayTypeSelector` into one location `app/javascript/PayTypeSelector`

For now, create the `app/javascript/PayTypeSelector/index.jsx` file
- This will contain a React component that renders the exact same HTML for the pay type drop down as our current Rails view

In this file, create the component
```javascript
import React from 'react'

class PayTypeSelector extends React.Component {
  render() {
    return (
      <div className='field'>
        <label htmlFor='order_pay_type'>Pay type</label>
        <select id='order_pay_type' name='order[pay_type]'>
          <option value=''>Select a payment method</option>
          <option value='Check'>Check</option>
          <option value='Credit card'>Credit card</option>
          <option value='Purchase order'>Purchase order</option>
        </select>
      </div>
    );
  }
}

export default PayTypeSelector
```
This looks like HTML but is actually JSX as it has some subtle deviations from HTML
- First, it must be well-formed XML, meaning each tag must either have a closing tag or be self closing (HTML does not require this: ex: <input> tags)
- Second, JSX cannot use JS keywords for attributes
  - We're using `className` and `htmlFor` where we'd usually use `class` and `for`, but these are reserved words in JavaScript
- We've also judiciously chosen the `name` value for `select` in exactly the same way a Rails form helper would
  - This will allow our controller to find the values, even though they are coming from a React component and not a Rails-rendered view
- The last line contains something new: `export`
  - This is the other side of `import`
  - In a Ruby, a file that is required via `require()` is simply executed
    - Any classes it creates are inserted into the global namespace
  - In JavaScript, you must explicitly state what you are exporting from your file

## 3. Bring the Component into the Rails view
Inside `app/views/orders/new.html.erb` add the javascript pack tag
`<%= javascript_pack_tag('pay_type') %>`

Next, remove the Rails-rendered pay type drop-down and add in a piece of markup with the id `pay-type-component` so the code inside our pack file can tell React to render there
- This is done in `app/views/orders/_form.html.erb`
- `<div id='pay-type-component'></div>`
  - The type of element doesn't actually matter since React will replace it, but a `div` is semantically appropriate


## Dynamically Replacing Components Based on User Action
React components render their views based on the state inside a component.  This means we need to capture the selected pay type as the component's state and render different form fields based on that state.

To detect events in plain JS, we'd add the `onchange` attribute to our `select` element, setting its value to JS code we'd like to execute.  This is the exact same in React, except its `onChange` with a capital C.
- We don't need to quote the value of `onChange`, we jsut use curly braces which allow us to inerpolate JS
- In the braces, we call a function `this.onPayTypeSelected`, which for now just logs the event target's value

The event passed is a **synthetic event** which has a property `target` that is a `DOMEventTarget`, which itself has a property `value` that has the value of the selected payment type

Since a React component is a view and state, when the state changes, we need to rerender the view
- Since we want the view to be rerendered when the user changes payment types, we need to get the currently selected payment type into the component's state
