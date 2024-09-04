`std::bind` is a way to create a wrapper that would call a given function with given parameters in any order, provide default values and transform them. All of the entities are defined in `<functional>`
```cpp
#include <functional>
#include <iostream>

void f(int a, int b, int c) {
    std::cout << a << ' ' << b << ' ' << c;
}

signed main() {
    auto bound = std::bind(f, std::placeholders::_2, 10, std::placeholders::_1);
    bound(1, 2);
}
```
The output is
```
2 10 1
```

Passing a reference wrapper would result in passing a reference itself. Passing a bind expression would result in eager evaluation and the return type is passed to the called function. The latter gives us the opportunity to transform given parameters, for example:
```cpp
#include <functional>
#include <iostream>

void f(int a, int b, int c) {
    std::cout << a << ' ' << b << ' ' << c;
}

int square(int x) {
    return x * x;
}

signed main() {
    auto bound = std::bind(f, std::bind(square, std::placeholders::_2), 10, std::placeholders::_1);
    bound(1, 2);
}
```
The output is
```
4 10 1
```

It is important to note, that it is impossible to pass a bound function into a bound function because of eager evaluation, but it is possible to pass a bound function as a parameter and use one placeholder multiple times. For example:
```cpp
#include <functional>
#include <iostream>

void f(auto a, int b, int c) {
    std::cout << a(b) << ' ' << b << ' ' << c;
    // a(b) = square(b, b) = 3 * 3 + 3 * 3 = 18
}

int square(int x, int y) {
    return x * x + y * y;
}

signed main() {
    f(std::bind(square, std::placeholders::_1, std::placeholders::_1), 3, 4);
}
```
The output is
```
18 3 4
```
