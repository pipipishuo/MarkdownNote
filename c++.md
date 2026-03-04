导出全局变量

在头文件中

```
__declspec(dllexport) extern int a;
```

cpp文件中

```
int a;
```



# 协程代码示例



```
#include <coroutine>
#include <iostream>
#include <vector>

template<typename T>
struct Generator {
    struct promise_type;
    using handle_type = std::coroutine_handle<promise_type>;

    struct promise_type {
        T value_;

        Generator get_return_object() {
            return Generator{ handle_type::from_promise(*this) };
        }
        std::suspend_always initial_suspend() { return {}; }
        std::suspend_always final_suspend() noexcept { return {}; }
        void unhandled_exception() {}

        std::suspend_always yield_value(T value) {
            value_ = value;
            return {};
        }
        void return_void() {}
    };

    handle_type handle_;

    explicit Generator(handle_type h) : handle_(h) {}
    ~Generator() { if (handle_) handle_.destroy(); }

    bool next() {
        if (!handle_.done()) {
            handle_.resume();
        }
        return !handle_.done();
    }

    T value() const { return handle_.promise().value_; }

    // 迭代器支持
    struct iterator {
        Generator& gen;
        bool done;

        iterator(Generator& g, bool d) : gen(g), done(d) {}

        iterator& operator++() {
            done = !gen.next();
            return *this;
        }

        bool operator!=(const iterator& other) const {
            return done != other.done;
        }

        T operator*() const { return gen.value(); }
    };

    iterator begin() { return iterator(*this, false); }
    iterator end() { return iterator(*this, true); }
};

Generator<int> fibonacci(int n) {
    int a = 0, b = 1;

    for (int i = 0; i < n; ++i) {
        co_yield a;
        int next = a + b;
        a = b;
        b = next;
    }
}

Generator<int> range(int start, int end, int step = 1) {
    for (int i = start; i < end; i += step) {
        co_yield i;
    }
}

int main() {
    std::cout << "Fibonacci: ";
    auto fib = fibonacci(10);
    while (fib.next()) {
        std::cout << fib.value() << " ";
    }
    std::cout << "\n";
    // 输出: 0 1 1 2 3 5 8 13 21 34

    std::cout << "Range: ";
    for (auto i : range(1, 10, 2)) {
        std::cout << i << " ";
    }
    // 输出: 1 3 5 7 9
}
```

