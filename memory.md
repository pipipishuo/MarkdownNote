参考链接 [Physical Memory — The Linux Kernel documentation](https://docs.kernel.org/mm/physical_memory.html#zones)

zones是按种类分的一个zone是一大块 看链接中的那个图

默认分配策略是按照离cpu最近的分

[pglist_data](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pglist_data) 是描述内存布局的 哦哦

[__MAX_NR_ZONES](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__MAX_NR_ZONES) 就是一个pglist_data中有几种zone  啊对了 就是这意思
