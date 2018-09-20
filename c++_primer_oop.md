object-oriented programming is based on three fundamental concepts: data abstraction, inheritance, and dynamic binding.

the key idea behind OOP is polymorphism.

polymorphism is derived from a Greek word meaning "many forms."

in C++, polymorphism applies only to references to or pointers to types related by inheritance.

对于返回对基类类型的引用（或指针）的虚函数，派生类中的虚函数可以返回基类函数所返回类型的派生类的引用（或指针）。

函数重载是指在同一作用域内，可以有一组具有相同函数名，不同参数列表的函数。

if we need to declare (but not yet define) a derived class, the declaration contain the class name but not include its derivation list. For example, the following forward declaration of `Bulk_item` results in compile-time error:

```c++
// error: a forward declaration must not include the declaration list.
class Bulk_item : public Item_base;
```

the correct forward declarations are:

```c++
class Bulk_item;
class Item_base;
```

binding a base-type reference or pointer to a derived object has no effect on the underlying object. The object itself is unchanged and remains a derived object. The actual type of the object might differ from the static type of the reference or pointer addressing that object.

non-virtual functions are always resolved at compile time based on the type of the object, reference, or pointer from which function is called.

when a derived virtual calls the base-class version, it must do so explicitly using the scope operator. If the derived function neglected to do so, then the call would be resolved at run time and would be a call to itself, resulting in an infinite recursion.

virtual functions and default arguments

when a virtual is called through a reference or pointer to base, then the default argument is the value specified in the declaration of the virtual in the base class. If a virtual is called though a pointer or reference to derived, the default argument is the one declared in the version in the derived class.

all classes that inherit from `Base` class have the same access to the member in `Base`, regardless of the access label in their derivation list. The derivation access label controls the access that users of the derived class have to members inherited from `Base`.

exempting individual members

when inheritance is `private` or `protected`, the access level of members of the base may be more restrictive in the derived class than it was in the base:

```c++
class Base
{
public:
    std::size_t size() const {return n;}
protected:
    std::size_t n;
};

class Derived : private Base { . . . };
```

the derived class can restore the access level of an inherited member. The access level can not be made more or less restrictive than the level originally specified within the base class.

in this hierarchy, `size` is `public` in `Base` but `private` in `Derived`. To make `size public` in `Derived` we can add a `using` declaration for it to a `public` section in `Derived`. By changing the definition of `Derived` as follows, we can make the `size` number accessible to users and `n` accessible to classes subsequently derived from `Derived`:

```c++
class Derived : public Base
{
public:
    // maintain access levels for member related to the size of the object
    using Base::size;
protected:
    using Base::n;
};
```

it a common misconception to think that there are deeper differences between classes defined using the `struct` keyword and those defined using `class`. The only differences are the default protection level for members and the default protection level for a derivation. There are no other distinction.

不存在空引用。

`RTTI`: `Run-Time Type Identification`

the operands to the `typeid` are expressions that are objects.

a class may initialize only its own immediate base class.