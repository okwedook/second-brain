### Atomicity
An operation is atomic, if two threads running this operation would run it sequentially.
A value is atomic, if all operations on it are atomic.

### `std::atomic`
`std::atomic` is a wrapper on C++ integral types (and some floating point types in newer standards). It provides some basic atomic operations as methods, such as `.fetch_add()`, `.fetch_xor()` and so on.
If there is only one (or a small amount) if shared variables in your program, it’s better to use atomic variables, than a full-scale mutex. Although, mutex uses a boolean atomic value (flag if mutex currently in use) inside of it and a thread pool would use an atomic int (the number of running threads). 

### Data race and `std::mutex`
When two threads want to write some data a data race might appear. It’s a state, when two threads do some non-atomic operations, which leads to data corruption. To cope with this problem we use the principle of **mut**ual **ex**clusion. The `mutex` protects (or guards) the critical section — code which is prone to data races.

The following code will fail miserably because of data races
```cpp
#include <future>
#include <iostream>

struct Account {
    int balance;
    Account(int _balance) : balance(_balance) {}
    bool spend(int amount) {
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
};

signed main() {
    const int B = 100000;
    Account account(B);
    auto spend = [&account]() {
        int spent = 0;
        for (int i = 0; i < B; ++i) {
            int amount = 1;
            if (account.spend(amount)) {
                spent += amount;
            }
        }
        return spent;
    };
    auto spender1 = std::async(spend);
    auto spender2 = std::async(spend);
    std::cout << spender1.get() + spender2.get() << std::endl;
    return 0;
}
```
```
Output: 186593
```

This is because `bool spend()` is a critical section
To protect it with `std::mutex` we need to alter the struct code to
```cpp
struct Account {
    int balance;
    std::mutex m;
    Account(int _balance) : balance(_balance) {}
    bool spend(int amount) {
        m.lock();
        if (balance >= amount) {
            balance -= amount;
            m.unlock();
            return true;
        }
        m.unlock();
        return false;
    }
};
```
```
Output: 100000
```
Here we lock the mutex before read/write operations and unlock it after them.

### `std::lock_guard`
The above code presents one of the bug prone features of `std::mutex` — we need to `.unlock()` it when we exit the scope, otherwise we can get a deadlock. To make less bugs it’s better to use `std::lock_guard`, which locks the mutex with its constructor and unlocks with its destructor.
```cpp
struct Account {
    int balance;
    std::mutex m;
    Account(int _balance) : balance(_balance) {}
    bool spend(int amount) {
        std::lock_guard<std::mutex> lock(m);
        if (balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
};
```
```
Output: 100000
```
It’s not necessary to write class template for `std::lock_guard`, so `std::lock_guard lock(m);` would work just fine.