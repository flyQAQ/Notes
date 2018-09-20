+ `static_cast`

  ```c
  // c
  float c = (float)a / (float)b;
  ```

  ```cpp
  // cpp
  float c = static_cast<float>(a) + static_cast<float>(b);
  ```

+ `const_cast`

  ```cpp
  int main() {
      int const a = 1;
      int b = const_cast<int>(a);
      b = 10;
      return 0;
  }
  ```

  Invalid use of `const_cast` with type `int`, which is not a pointer,  reference, or a pointer-to-data-member type.

+ `reinterpret_cast`

  ```cpp
  int i;
  float f = -6.9072;
  unsigned char *p = reinterpret_cast<unsigned char *>(&f);
  cout << hex;
  for (int i = 0; i < sizeof(float); ++i) {
      cout << static_cast<int>(p[i]) << endl;
  }
  ```

+ `dynamic_cast`

  The RTTI operators execute at run time for classes with virtual functions, but are evaluated at compile time for all other types.