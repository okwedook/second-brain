### lvalue and rvalue
**lvalue** is something, that has a location
**rvalue** is something, that has temporary storage
```cpp
// lvalue = rvalue
int a = 10;
// rvalue = lvalue
10 = a; // ERROR! Can't assign to a rvalue

int getValue() {
    return 10;
}
// lvalue = rvalue
int a = getValue();
```

### References
However, a function might return a **lvalue** reference, which is totally ok
```cpp
int& getValue() {
    static int x = 10;
    return x;
}
getValue() = 15;
print(getValue()); // prints 15
```
Now, when we pass an argument to a function, there are multiple ways to do it: by value, by reference (**lvalue**), by const reference (both **lvalue** and **rvalue**) and **rvalue** reference.
```cpp
void func(int value) {}
void func(int &value) {}
void func(const int &value) {}
void func(int &&value) {}
```
While lvalue references are fairly straightforward an **rvalue** reference gives us the possibility to own the underlying data. A **rvalue** reference is going to work only with temporary or moved objects.

### Move semantics
C++ introduced some improvements on moving objects around in C++11. So, `std::move` basically converts an object into a `rvalue` reference. This allows us to copy arguments without physically moving them.
A good example of moving a vector into a struct field
```cpp
struct Class {
    vector<int> v;
    Class(const vector<int> &_v) : v(_v) {
        println("Copy constructor");
    }
    Class(vector<int> &&_v) : v(move(_v)) {
        println("Move constructor");
    }
};

vector<int> a = {1, 2, 3};
vector<int> b = {4, 5};
Class c(a); // prints "Copy constructor"
Class d(move(b)); // prints "Move constructor"
println(a.size()); // prints 3, since a didn't change
println(b.size()); // prints 0, since move constructor cleared b
```
