### `std::thread`
The C++ way of using threads is `#include <thread>`

This is a basic example of a program with two threads. Also it has an example of how to run a function in a different thread with parameters.

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std::chrono_literals;

void function(int len, char symbol) {
    for (int i = 0; i < len; ++i) {
        std::cout << symbol << std::flush;
    }
}

signed main() {
    std::thread thread1(function, 10000, '+');
    std::thread thread2(function, 10000, '-');
    thread1.join();
    thread2.join();
    return 0;
}
```


### `join` vs `detach`
The difference between `join` and `detach` is fairly simple. When a thread is `join`ed the upper thread is awaiting it’s end, but when `detach`ed, the program might exit out of scope and let the thread run by itself. `detach` is useful, when we want to let some process finish, but we do not necessarily need the result of the process to continue working. A good example here would be logging or generally writing to a file, updating counters and so on. It’s important to acknowledge, that the program might be terminated before the end of the detached threads, in which case all of it’s daughter threads would be terminated too, so in the example above detaching the threads would most likely result in zero output.

### `std::async` and `std::future`
`std::async` runs a function asynchronously and returns an `std::future` as it’s result. Then one can await the result of the `future` for some time or until finished and get the result. Waiting for the result will block the thread.
The following code showcases basic usage
```cpp
#include "timeit.h"
#include <iostream>
#include <future>
#include <vector>

using ll = long long;
const int MOD = 1000000007;

int fact(int n) {
    int ans = 1;
    for (int i = 1; i <= n; ++i) {
        ans = (ans * ll(i)) % MOD;
    }
    return ans;
}

std::vector<int> facts(int l, int r) {
    std::vector<int> ans(r - l);
    for (int i = l; i < r; ++i) {
        ans[i - l] = fact(i);
    }
    return ans;
}

signed main() {
    const int N = 50000;
    {
        TIMEIT(Single thread);
        std::vector<int> ans;
        for (int i = 1; i <= N; ++i) {
            ans.push_back(fact(i));
        }
    }
    {
        TIMEIT(Multiple threads);
        const int page_size = 40;
        std::vector<std::future<std::vector<int>>> answers;
        for (int i = 0; i < N; i += page_size) {
            answers.push_back(
                async(facts, i, std::min(N, i + page_size))
            );
        }
        std::vector<int> ans;
        for (auto &answer : answers) {
            for (auto i : answer.get()) {
                ans.push_back(i);
            }
        }
    }
    return 0;
}
```
The output is
```
[Single thread]: 5.13861
[Multiple threads]: 0.998471
```

`std::async` has different policies with which to run the underlying asynchronous functions. The default values are `std::launch::async`, which enables asynchronous evaluation and `std::launch::deferred`, which enables lazy evaluation.

It’s important to remeber, that `std::future` awaits it’s result in the destructor, so the following code is actually ran sequentially instead of in parallel
```cpp
async(f, 1);
async(f, 2);
```

###  `std::packaged_task` and `std::promise`
TODO

### Thread pools
Although there is no STL implementation of a thread pool (to my best knowledge), this abstraction is important to know. One can use `boost::asio::thread_pool` for it. At each moment in time there would be at most `threadNumber` threads running and we can send new jobs (asynchronous functions) for it to process.

[Mutex](Mutex.md)