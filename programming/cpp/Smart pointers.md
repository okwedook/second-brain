Smart pointers are wrappers on raw C++ pointers, which delete the data automatically when needed, depending on the type of smart pointer.

### `unique_ptr`
Simplest of smart pointers. It’s a scope based pointer, meaning it’s memory is going to be freed at the end of the scope. So `unique_ptr` is not a copyable object.

```cpp

int* raw_ptr = new int(10);
print(*raw_ptr); // Prints 10
{
    unique_ptr<int> st(raw_ptr);
    print(*st); // Prints 10
    *st = 20;
    print(*st); // Prints 20
    unique_ptr<int> copied_st = st; // Wouldn't work
}
print(*raw_ptr); // Prints trash, UB
delete raw_ptr; // ERROR! Attempting double-free
```
Though the pointer is not copyable, it’s still movable, since there is only one copy and it’s in scope. So this code works
```cpp
unique_ptr<int> get_unique(int x) {
    return unique_ptr<int>(new int(x));
}

signed main() {
    auto ptr = get_unique(10);
    auto ptr_moved = move(ptr);
    print(*ptr_moved);
    auto ptr_copied = ptr_moved; // ERROR! Trying to copy a unique_ptr
    return 0;
}
```

### `shared_ptr`
The difference between a `unique_ptr` and a `shared_ptr` is that the memory could be used by many pointers at once. It uses reference counting under the hood, which is basically the number of `shared_ptr`s which point to this object.
So this code is now working
```cpp
int* raw_ptr = new int(20);
shared_ptr<int> out_scope(raw_ptr);
print(*out_scope); // prints 20
{
    shared_ptr<int> ptr(new int(10));
    print(*raw_ptr); // prints 20
    print(*ptr); // prints 10
    out_scope = ptr;
    // raw_ptr memory is deleted after this assignment
    print(*raw_ptr); // Prints trash, UB 
}
print(*out_scope); // prints 10
```

### `weak_ptr`
A `weak_ptr` is a shared pointer which doesn’t increase the reference counter  
So we can’t really create a `weak_ptr` from a raw pointer or a `unique_ptr`
```cpp
weak_ptr<int> wptr(new int(20)); // ERROR! No such constructor
auto uptr = make_unique<int>(10);
weak_ptr<int> wptr = uptr; // ERROR! Can't copy a unique_ptr
```
But we can copy `shared_ptr`. To access the underlying data we need to construct a `shared_ptr` with `.lock()`
```cpp
auto sptr = make_shared<int>(20);
weak_ptr<int> wptr = sptr;
{
    shared_ptr<int> sptr = wptr.lock();
    print(*sptr); // prints 20
}
{
    auto sptr = make_shared<int>(10);
    wptr = sptr;
    // Memory is deleted with sptr's constructor
}
{
    shared_ptr<int> sptr = wptr.lock();
    print(*sptr); // prints trash, UB
}
```

### Constructor functions
To avoid writing `new` at all costs we can use constructor functions to create new objects. 
```cpp
auto uptr = make_unique<int>(10);
auto sptr = make_shared<int>(20);
```
This helps handling exceptions if they happen in constructors