`C++` defines a set of **arithmetic types**, which represent integers, floating-point numbers, and individual characters and `boolean` values. In addition, there is a special type named `void`.

The arithmetic types that represent integers, characters, and `boolean` values are collectively referred to as the **integral types**.

When assigning an out-of-range value to a `signed` type, It is up to the compiler to decide what value to assign.

A variable provides us with named storage that our program can manipulate.

Initialization happens when a variable is created and gives that variable its initial value. Assignment involves obliterating an object's current value and replacing that value with a new one.

Whether a variable of build-in type is automatically initialized depends on where it is defined. Variables defined outside any function body are initialized to zero. Variable of build-in type defined inside the body of a function are uninitialized.

```c++
void function()
{
    // compile-error, 使用了未初始化的局部变量 a.
    int a;
    std::cout << a << std::endl;
}
```

A **definition** of a variable allocates storage for the variable and may also specify an initial value for the variable. **There must be one and only one definition of a variable in program**.

A **declaration** makes known the type and name of the variable to the program.

```c++
extern int i; // declares but does not define i
int i; // declares and difines i
```

An `extern` declaration may include an initializer only if it appears outside a function.

**global scope**, **local scope**, **statement scope**

`const` object are local to a file by default.

Non-`const` variable are `extern` by default. To make a `const` variable accessible to other files we must explicitly that it is `extern`.

We cannot define a reference to a reference type.

A non-`const` reference may be attached only to an object of the same type as the reference itself. A `const` reference may be bound to an object of a different but related type to an are-value.

`typedef` are commonly used for one of three purposes:

+ To hide the implementation of a given type an emphasize instead the the purpose for which the type is used.
+ To streamline complex type definitions, making them easier to understand.
+ To allow a single type to be used for more than on purpose while making the purpose clear each time the type is used.

A constant expression is an expression of integral type that the compiler can evaluate at compile time.

There is one crucially important difference between how we define variables and class members: We ordinarily cannot initialize the members of class as part of their definition. When we define the data members, we can only name them and say what types they have. Rather than initializing data members when they are defined inside the class definition, class control initialization through special member functions called constructors.