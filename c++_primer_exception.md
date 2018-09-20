An exception is raised by throwing an object. The type of that object determines which handler will be invoked. The selected handler is the one nearest in the call chain that matches the type of the object.

###### Finding A Matching handler

During the search for a matching `catch`, the `catch` that is found is not necessary the one that matches the exception best. Instead, the `match` that is selected is the first `catch` found that can be handle the exception. As a consequence, in o list of `catch` clauses, the most specialized `catch` must appear first.

The rule for when an exception matches a `catch` exception `specifier` are much more restrictive than the rule used for matching arguments with parameter types. Most conventions are not allowed, the type of exception and the `catch specifier` must match exactly with only a few possible difference:

+ Conventions from `non-const` to `const` are allowed. That is, a throw of a `non-const` object can match a `catch` specified to take a `const` reference.
+ Conventions from derived type to base type are allowed.
+ An array is converted to a pointer to the type of the array; a function is converted to the appropriate pointer to function type.