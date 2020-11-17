# MVC Web Applications
- Models responsible for maintaining the state of the application
  - More than just data: models enforce the business rules that apply to that data
  - By placing data constraints on the model, we know that no other part of our app can make our data invalid
- Views responsible for generating a user interface, normally based on data in the model
- Controllers orchestrate the application
  - Receive events from the outside world, interact with the model, and display an appropriate view to the user

# Rails Model Support

## Object-Relational Mapping (ORM)
- ORM libraries map database tables to classes
  - Within an object, attributs are used to get and set individual columns
- Rails classes that wrap our database tables provide a set of class-level methods that perform table-level operations (`.find`, `.where`, etc.)
- AciveRecord is the ORM layer supplied with Rails
  - Closely follows the standard ORM model: tables map to classes, rows to objects, columns to object attributes

## Action Pack: The View and Controller
- ActionPack is the single component that supports views and controllers

### View Support
- The View is responsible for creating all or part of a response to be displayed by the browser
- Supports HTML (and ERB), XML, and JSON

## The Controller
- The logical center of the application
- Coordinates the interaction among the user, views, and model
- Controller also home to several important ancillary services
  - Responsible for routing external requests to internal actions
  - Manages caching, which can give applications orders-of-magnitude performance boosts
  - Manages helper modules, which extend the capabilities of the view templates without bulking up their code
  - Manages sessions, giving users the impression of ongoing interaction with our application