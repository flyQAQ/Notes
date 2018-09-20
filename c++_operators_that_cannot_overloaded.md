##### Operators That Cannot Be Overloaded

1. `::` scope resolution

2. `.` member selection

3. `.*` member selection through pointer

   These three operators take a variable name rather than a value as their second operand and provide the primary means of referring to members.So syntactically these operators can not be overridden.

4. `?:` ternary or conditional operator

5. `sizeof` operator

   the `sizeof` operator returns the size of the object /type passed as an operand. It is evaluated by the compiler not at runtime, so you can not overload it with your own runtime code. It is syntactically not possible to do. 

6. `typeid` object type operator

   For `typeid`, for instance, the whole point is to uniquely identify a type. If you’re trying to make a user-defined type “look” like another type, you’re better off taking advantage of polymorphism and/or type-conversion  semantics.