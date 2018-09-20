###### Unnamed Namespace

+ An unnamed namespace may be `discontiguous` within a given file but does not span files. Each file has its own unnamed namespace.

+ Unnamed namespace are used to declare entities that are local to a file. Variables defined in an unnamed namespace are created when the program is started and exist until the program ends.

+ Names defined in an unnamed namespace are used directly; after all, there is no namespace name with which to qualify them. It is not impossible to use the scope operator to refer to members of unnamed namespaces.

+ Names defined in an unnamed namespace are found in the same scope as the scope at which the namespace is defined. If an unnamed namespace is defined at the outermost scope in the file, then names in the unnamed namespace must differ from names defined at global scope.

  ```cpp
  int i; // global declaration for i
  namespace {
      int i;
  }
  ::i = 0;
    i = 0; // error: ambiguous defined globally and in an unnested, unnamed namespace
  ```

+ An unnamed namespace may be nested inside another namespace.

  ```cpp
  int i; // global declaration for i

  namespace local {
      namespace {
          int i;
      }
  }

  // ok: i defined in a nested unnamed namespace is distinct from global
  local::i = 0;
  ```

  ​

```cpp
int a = 0;

namespace {
    int a = 0;
}

::a = 1; // global
```

```cpp
namespace {
    int a = 0;
}

::a = 1; // unnamed namespace
```

