导出全局变量

在头文件中

```
__declspec(dllexport) extern int a;
```

cpp文件中

```
int a;
```



<<<<<<< HEAD
# 关于协程的理解

示例代码


```
#include <coroutine>
#include <iostream>
<<<<<<< HEAD
#include <exception>
#include <thread>

// 一个简单的Generator模板

struct Generator {
    // promise_type 内部定义
    struct promise_type {
        int current_value; // 存储通过 co_yield 产生的值

        // 开始时不挂起，让生成器立即执行到第一个yield
        std::suspend_never initial_suspend() { return {}; }
        // 最终挂起，以便我们可以在外部通过 done() 检查是否结束
        std::suspend_always final_suspend() noexcept { return {}; }

        // 获取返回对象，并将当前promise的句柄传递给Generator
        Generator get_return_object() {
            return Generator{ std::coroutine_handle<promise_type>::from_promise(*this) };
        }

        // 处理 co_yield
        std::suspend_always yield_value(int value) {
            current_value = value;      // 保存值
            return {};                   // 挂起协程，将值返回给调用者
        }

        // 因为没有使用 co_return 返回值，所以这里 return_void 即可
        void return_void() {}
        void unhandled_exception() { std::terminate(); }
    };

    // 协程句柄，用于控制关联的协程
    std::coroutine_handle<promise_type> coro_handle;

    // 构造函数，接收句柄
    Generator(std::coroutine_handle<promise_type> h) : coro_handle(h) {}
    // 析构函数，销毁协程帧，防止内存泄漏
    ~Generator() { if (coro_handle) coro_handle.destroy(); }

    // 禁止拷贝
    Generator(const Generator&) = delete;
    Generator& operator=(const Generator&) = delete;

    // 允许移动
    Generator(Generator&& other) noexcept : coro_handle(other.coro_handle) {
        other.coro_handle = nullptr;
    }

    // 获取下一个值
    int next() {
        coro_handle.resume(); // 恢复协程，让它执行到下一个 co_yield
        return coro_handle.promise().current_value; // 返回保存的值
    }

    // 检查是否执行完毕
    bool done() const {
        return coro_handle.done();
    }
};
struct SimpleAwaiter {
    std::coroutine_handle<> waiter;
    int result;

    bool await_ready() const {
        std::cout << "检查操作是否已完成" << std::endl;
        return false;  // 假设总是未完成
    }

    void await_suspend(std::coroutine_handle<> handle) {
        std::cout << "协程挂起，保存句柄" << std::endl;
        waiter = handle;

        // 在新线程中模拟异步操作
        std::thread([this] {
            std::this_thread::sleep_for(std::chrono::seconds(1));
            result = 42;
            std::cout << "异步操作完成，恢复协程" << std::endl;
            waiter.resume();  // 恢复协程
            }).detach();
    }

    int await_resume() {
        std::cout << "协程恢复，返回结果" << std::endl;
        return result;
    }
};
// 一个协程函数，用于生成从 start 开始的 count 个整数
Generator range(int start, int count) {
    for (int i = start; i < start + count; ++i) {
        printf("sssssssssss\n");
    }
    co_yield 3; // 产生一个值并挂起
    printf("resume\n");
    co_await SimpleAwaiter{};
}
struct TEST {
    int a;
    TEST(int a) {

    }
};
int main() {
    Generator gen = range(1, 5); // 创建生成器
    TEST t = 2;
    // 循环获取值，直到生成器结束
    while (!gen.done()) {
        std::cout << gen.next() << " "; // 输出: 1 2 3 4 5
    }
    std::cout << std::endl;

    return 0;
}
```



协程是依托于结构体的

你得在结构体里面声明一个std::coroutine_handle<promise_type>成员

```
struct promise_type 

// 协程句柄，用于控制关联的协程
std::coroutine_handle<promise_type> coro_handle;

// 构造函数，接收句柄
Generator(std::coroutine_handle<promise_type> h) : coro_handle(h) {}
```

其中promise_type是很重要的类型  就得叫这个名  叫别的都不好使  这是约定

而且必须得在Generator里面创建  这个结构体相比较一般的结构体应该是多了一个步骤的编译 但也是结构体

