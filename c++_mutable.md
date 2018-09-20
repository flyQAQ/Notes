##### Mutable Lambdas

Object captured by value in lambda are immutable by default. This is because `operator()` of the generated functional object (compiler generates functional object for lambdas) is `const` by default.

```c++
int a = 0;
auto lam = [a]() { ++a; }; // Compilation error
```

Using `mutable` keyword allows us to mutate objects captured by value.

```c++
int a = 0;
auto lam = [a]() mutable { ++a; };
```

###### The Parts Of Lambda

1. `capture clause.`
2. `parameter list Optional.`
3. `mutable specification Optional.`
4. `exception specification Optional.`
5. `trailing-return-type Optional.`
6. `lambda body.`