# io_ctx

很好的示例

```
#include <iostream>
#include<boost/asio.hpp>
#include<thread>
void multi_thread_io_ctx() {
    boost::asio::io_context io_ctx;
    boost::asio::executor_work_guard<boost::asio::io_context::executor_type>
        work_guard = boost::asio::make_work_guard(io_ctx);
    std::atomic<int> completed_count{0};
    const int total_tasks = 10;
    for (int i = 0; i < total_tasks; i++) {
        boost::asio::post(io_ctx, [i, &completed_count]() {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            std::cout << "任务 " << i << " 在线程 "
                << std::this_thread::get_id() << " 完成\n";
            ++completed_count;
            });
    }

    std::vector<std::thread> threads;
    const size_t num_thread = std::thread::hardware_concurrency();

    for (size_t i = 0; i < num_thread; i++) {
        threads.emplace_back([&io_ctx]() {
            io_ctx.run();
            });
    }
    // 等待所有任务完成
    while (completed_count < total_tasks) {
        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
    // 停止工作并等待线程结束
    work_guard.reset();  // 允许 io_context 停止
    io_ctx.stop();       // 停止 io_context

    for (auto& t : threads) {
        if (t.joinable()) t.join();
    }
}
int main()
{
    multi_thread_io_ctx();
    boost::asio::io_context io_ctx;
    boost::asio::steady_timer timer1(io_ctx,std::chrono::seconds(1));
    boost::asio::steady_timer timer2(io_ctx, std::chrono::seconds(3));
    timer1.async_wait([](const boost::system::error_code& ec) {
        if (!ec) {
            std::cout << "定时器1: 1秒后触发\n";
        }
        });
    timer2.async_wait([](const boost::system::error_code& ec) {
        if (!ec) {
            std::cout << "定时器2: 2秒后触发\n";
        }
        });
    io_ctx.run();
    std::cout << "Hello World!\n";
}


```

