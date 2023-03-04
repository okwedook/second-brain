Some code snippets will use the following macro to measure performance of given code
```cpp
#pragma once
#include <chrono>
#include <string>
#include <iostream>

struct Timeit {
    using time_point = std::chrono::system_clock::time_point;
    time_point start;
    std::string msg;
    Timeit(const std::string &_msg) : msg(_msg) {
        start = std::chrono::high_resolution_clock::now();
    }
    ~Timeit() {
        time_point end = std::chrono::high_resolution_clock::now();
        auto diff = end - start;
        std::cout << "[" << msg << "]: " << diff.count() * 1.0 / 1000000000 << std::endl;
    }
};

#define TIMEIT(msg) Timeit timeit__LINE__(#msg)
```