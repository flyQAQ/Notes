> A `void *` indicates that the associated value is an address but that the type of the object at that address is unknown.

> `std::nullptr_t`
>
> ```cpp
> typedef decltype(nullptr) nullptr_t;
> ```

##### Managing Pointer Members (Section 13.5)

1. The pointer member can be given normal `pointer-like` behavior. Such class will have all the pitfalls of pointers but will require no special copy control.
2. The class can implement so-called `"smart pointer"` behavior. The Object to which the pointer points is shared, but the class prevents dangling pointers.
3. The class can be given `value-like` behavior. The object to which the pointer points will be unique to and managed separately by each class object.

###### A Simple Class With A Pointer Member

```cpp
class HasPtr {
public:
    // copy of the value we're given
    HasPtr(int *p, int i): ptr(p), val(i) { }
    // const members to return the value of the indicated data member
    int *get_ptr() const { return ptr; }
    int  get_int() const { return val; }

    // non const members to change the indicated data member
    void set_ptr(int *p) { ptr = p; }
    void set_int(int i) { val = i; }

    // return or change the value pointed to, so ok for const objects
    int  get_ptr_val() const { return *ptr; }
    void set_ptr_val(int val) const { *ptr = val; }
private:
    int *ptr;
    int  val;
};
```

> Classes that have pointer members and use default synthesized copy control have all the pitfalls of ordinary pointers. In particular, the class itself has no way to avoid dangling pointers.

###### Dangling Pointers Are Possible

```cpp
int *ip = new int(42);
HasPtr ptr(ip, 10);
delete ip;
ptr.set_ptr_val(0); // disaster: The object to which HasPtr points was freed.
```

###### Defining Smart Pointer Classes

Our new `HasPtr` class will need a destructor to delete the pointer. However, the destructor cannot delete the pointer unconditionally. If two `HasPtr` objects point to the same underlying object, we don't want to delete the object until both objects are destroyed. To write the destructor, we need to know whether this `HasPtr` is the last one pointing to a given object.

###### Introducing Use Counts

A common technique used in defining smart pointers is to use a use count. The `pointer-like` class associates a counter with the object to which the class points. The use count keeps track of how many objects of the class share the same pointer. When the use count goes to zero, then the object is deleted. A use count is sometimes also referred to as a reference count.

The only wrinkle is deciding where to put the use count. The counter cannot go directly into our `HasPtr` object.

###### The Use-Count Class

There are two classic strategies for implementing a use count, one of which we will use here. In the approach we use here, we will define a separate concrete class to encapsulate the use count and the associated pointer:

```cpp
class U_Ptr {
    friend class HasPtr;
    int *ip;
    size_t use;
    U_Ptr(int *p): ip(p), use(1) { }
    ~U_Ptr() { delete ip; }
};
```

We don't intend ordinary users to use the `U_Ptr` class, so we do not give it any `public` members.

###### Using The Use-Count Class

Our new `HasPtr` class holds a pointer to a `U_Ptr`, which in turn points to the actual underlying `int` object. Each member must be changed to reflect the fact that the class points to a `U_Ptr` rather than an `int *`.

```cpp
class HasPtr {
public:
    HasPtr(int *p, int i): ptr(new U_Ptr(p)), val(i) { }
    
    // copy control
    HasPtr(const HasPtr &orig): ptr(orig.ptr), val(orig.val) { 
        ++ptr->use;
    }
    HasPtr& operator=(const HasPtr&);
    
    // if use count goes to zero, delete the U_Ptr object
    ~Has_Ptr() { if (--ptr->use == 0) delete ptr; }
private:
    U_Ptr *ptr;
    int val;
};
```

###### Assignment And Use Counts

This assignment operator guards against self-assignment by increasing the use count of `rhs` before decrementing the use count of the left-hand operand.

```cpp
HasPtr& HasPtr::operator=(const HasPtr &rhs) {
    ++rhs.ptr->use;
    if (--ptr->use == 0) {
        delete ptr;
    }
    ptr = rhs.ptr;
    val = rhs.val;
    return *this;
}
```

###### changing other Members

```cpp
class HasPtr {
public:
    int *get_ptr() const { return ptr->ip; }
    int  get_val() const { return val; }
    
    void set_ptr(int *p) { ptr->ip = p; }
    void set_val(int  i) { val = i; }
    
    int get_ptr_val() const { return *ptr->ip; }
    void set_ptr_val(int i) const { *ptr->ip = i; }
};
```

##### Defining Value-Like Classes

To make our pointer behave like a value, we must copy the object to which the pointer points whenever we copy the `HasPtr` object:

```cpp
class HasPtr {
public:
    HasPtr(const int &p, int i): ptr(new int(p)), val(i) { }
    HasPtr(const int *p, int i): ptr(new int(*p)), val(i) { }
    
    // copy control
    HasPtr(const HasPtr &orig)
        : ptr(new int(*orig.ptr)), val(orig.val) { }
    HasPtr& operator=(const HasPtr&);
    ~HasPtr() { delete ptr; }
private:
    int *ptr;
    int  val;
};
```

The copy constructor no longer copies the pointer. It now allocates a new `int` object and initializes that object to hold the same value as the object of which it is a copy. Each object always holds its own, distinct copy of its `int` value. Because each object holds its own copy, the destructor unconditionally deletes the pointer.

The assignment operator doesn't need to allocate a new object. It just has to remember to assign a new value to the object to which its `int` pointer points rather than assigning to the pointer itself:

```cpp
HasPtr& HasPtr::operator=(const HasPtr &rhs) {
    *ptr = *rhs.ptr;
     val =  rhs.val;
    return *this;
}
```

In other words, we change the value pointed to but not the pointer.

> As Always, the assignment operator must be correct even if we'are assigning an object to itself. In this case, the operations are inherently safe even if the left-hand and right-hand objects are the same. Thus, there is no need to explicitly check for self-assignment.

##### Handle Classes & inheritance

One of the ironies of object-oriented programming in `c++` is that we cannot use object to support it. Instead, we must use pointers and references, not objects. 

Unfortunately, using pointers or references put a burden on users of our classes.

A common technique in C++ is to define a so-called cover or handle class. The handle class stores and manages a pointer to the base class. The type of the object to which that pointer points will vary; it can point at either a base- or a derived-type object. Users access the operations of the inheritance hierarchy through the handle. Because the handle uses its pointer to execute those operations, the behavior of virtual members will vary at run time depending on the kind of object to which the handle is actually bound. Users of the handle thus obtain dynamic behavior but do not themselves have to worry about managing the pointer.

Handles that cover an inheritance hierarchy have two important design considerations:

+ As with any class that holds a pointer, we must decide what to do about copy control. Handles that cover an inheritance hierarchy typically behave like either a smart pointer or a value.
+ The handle class determines whether the handle interface will hide the inheritance hierarchy or expose it. If the hierarchy is not hidden, users must know about and use objects in the underlying hierarchy.