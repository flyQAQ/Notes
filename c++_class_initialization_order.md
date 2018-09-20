The following set of rules applies recursively:

- First, the most derived class's constructor calls the constructors of the virtual base class sub-objects. Virtual base classes are initialized in depth-first, left-to-right order.
- Next, direct base class sub-objects are constructed in the order they declared in the class definition.
- Next, non-static member sub-object are constructed in the order they were declared in class definition.
- Finally, the body of the constructor is executed.

Whether the inheritance is public, protected, or private doesn't affect initialization order.