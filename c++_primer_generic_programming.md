generic programming involves writing code in a way that is independent of any particular type.

###### Template

+ Specialization.
+ Partial Specialization.
+ Default Template  Arguments.
+ Non-type Template Parameters (must be constant expression, optional cv-qualified).
  + integral or enumeration type
  + pointer to object or pointer to function
  + `lvalue` reference to object or `lvalue` reference to function
  + pointer to member
  + `std::nullptr_t`

A class template can be partially specialized and/or fully specialized. A function template can only be fully specialized, but function templates can overload.

Specialized is not overload.

Compiler does not compile template functions that are never used, it only checks their syntax.