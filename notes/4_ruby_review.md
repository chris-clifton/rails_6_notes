# 4: Ruby Review

## Modules
- Serve two purposes
  1. Namespacing
    - Lets you define methods whose name wont clash with those defined elsewhere
  2. Sharing functionality across classes
- If a class mixes in a module, that module's methods become available as if they had been defined in the class
- Multiple classes can mixin the same module, sharing the module's functionality without using inheritance
- Can mix multiple modules into a single class

### Helper Methods
- Rails uses modules to create helper methods
- These helper modules are automatically mixed into the appropriate view templates

### YAML
- YAML (YAML Aint Markup Lanugage) is a module thats part of the Ruby standard library
- Allows convenient configuration of things such as databases, test data, and translations

## Marshalling Objects
- Ruby can take an object and convert it into a stream of bytes that can be stored outside the application
- The saved object can later be read by another instance of the application (or by a totally separate application), and a copy of the originally saved object can be reconstituted

### Two problems with marshalling
1. Some objects cant be dumped  
- If the objects include bindings, procedure or method objects, instances of the IO class, or singleton objects, a TypeError will be raised
2. When you load a marshalled object, Ruby needs to know the definition of the class of that object (and all the objects it contains)

## Rails and Marshalling
- Rails uses marshalling for session data