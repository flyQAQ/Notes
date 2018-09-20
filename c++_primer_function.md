function overload resolution (also known as function matching) is the process by which a function call is associated with a specific function from a set of overloaded functions.

there are three possible outcomes:

1. the compiler finds one function that is a best match for the actual arguments and generates code to call that function.

2. there is no function with parameters that match the arguments in the call, in which case the compiler indicates a compile-time error.

3. there is more than one function that matches and none of the matches is clearly best. This case is also an error; the call is ambiguous.

three steps in overload resolution:

1. candidate functions.

2. determining the viable functions.

3. find the best match, if any.

in order to determine the best match, the compiler ranks the conversions that could be used to convert each argument to the type of its corresponding parameter. Conversions are ranked in descending order as follows:

1. an exact match. The argument and parameter types are the same.

2. match through a promotion.

3. match through a standard conversion.

4. match through a class-type conversion.

if the return type and parameter list of two functions declarations match exactly, then the second declaration is treated as redeclaration of the first. If the parameter lists of two functions match exactly but the return types differ, then the second declaration is an error.

`void function(int);`
`void function(const int)`

in this pair, the differs only as to whether the parameter is const. This difference has no effect on the object that can be passed to the function; the second declaration is treated as a redeclaretion of the first. The reason follow how arguments are passed. When the parameter is copied, whether the parameter is const is irrelevantthe function executes on a copy. Nothing the function does can change the argument. As a result, we can pass a const object to either a const or a non-const paramter. The two parameters are indistinguishable.

a name declared local to a function hides the same name declared in the global scope. If we declare a function locally, that function hides rather than overloads the same function declared in an outer scope.

if the parameter is a plain reference, then we can may not pass a const object for that parameter. If we pass a const object, then the only function that is viable is the version that takes a const reference.

a class cannot have data members of its own type.

An incomplete type can be used only in limited ways. Objects of the type may not be defined. An incomplete type may be used to define only pointers or references to the type or to declare (but not define) functions that use the type as a paremeter or return type.

we can overload a member function based on whether it is const for the same reasons that we can overload a function based on whether a pointer parameter points to const.