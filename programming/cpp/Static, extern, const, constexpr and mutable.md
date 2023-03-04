### `static` in a struct/class
A static variable/method is single for all the entities of a class. Changing a static variable will change its value for all entities. A static method doesn’t need an entity to run at all.

```cpp
struct Class {
    static int x;

    static void update_x() {
        ++x;
    }
};

int Class::x = 20; // Has to be defined out of class

signed main() {
    Class e1;
    Class e2;
    println(e1.x); // prints 20
    e1.update_x();
    Class::update_x(); // Can be used like namespace
    println(Class::x); // prints 22
    return 0;
}
```

### `static` outside of struct/class
This makes a variable/function available only in the given translation unit (source file). To get link the variable/function at linker one would use the `extern` keyword.

### `const` variables
`const` is a keyword, which is a promise, that a variable is not going to be changed in the scope.     The compiler wouldn’t let you change the value of that variable.
```cpp
const int a = 10;
a = 11; // ERROR! Can't assign to a const value
```

### `const` pointers
There is a slight difference with raw pointers though. The pointer itself might be `const` or the underlying data might be `const`. Here is map from raw pointers to [smart pointers](Smart%20pointers.md)
1. `int*` = `unique_ptr<int>`
1. `const int*` = `int const*` = `unique_ptr<const int>`
1. `int* const` = `const unique_ptr<int>`
1. `const int* const` = `const int const*` = `const unique_ptr<const int>`
In the second and fourth case the underlying data is `const` and in third and fourth case the pointer itself is `const`.

### `const` method
Declaring a method `const` is restricting changing all the fields in a class. This lets us using this method when we pass the class entity as a const reference.
```cpp
struct Class {
    int x;
    // All correct
    int getter() const {
        return x;
    }
    // Changes the field
    void setter(int new_value) const {
        x = new_value; // ERROR! Assigning a read-only object
    }
};
```

### `mutable` keyword
If you really need to change some field in a class, you can mark it as `mutable`.  So the following would actually work
```cpp
struct Class {
    mutable int x;
    // No error here, since x is now mutable
    void setter(int new_value) const {
        x = new_value;
    }
};
```

### `constexpr` keyword
`constexpr` lets us calculate the value of some functions at compile time. A simple example would be
```cpp
constexpr int add(int a, int b) { return a + b; }

print(add(1, 2));
// add(1, 2) will always return 3
// so this value would be computed at compile time
int a, b;
read(a, b);
print(add(a, b)); // Computed at runtime
```