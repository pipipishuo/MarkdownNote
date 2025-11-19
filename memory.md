参考链接 [Physical Memory — The Linux Kernel documentation](https://docs.kernel.org/mm/physical_memory.html#zones)

zones是按种类分的一个zone是一大块 看链接中的那个图

默认分配策略是按照离cpu最近的分

[pglist_data](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pglist_data) 是描述内存布局的 哦哦

[__MAX_NR_ZONES](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__MAX_NR_ZONES) 就是一个pglist_data中有几种zone  啊对了 就是这意思

这个宏 定义了folio_test_slab的操作

```
#define FOLIO_TYPE_OPS(lname, fname)                    \
static __always_inline bool folio_test_##fname(const struct folio *folio) \
{                                    \
    return data_race(folio->page.page_type >> 24) == PGTY_##lname;    \
}                                    \
static __always_inline void __folio_set_##fname(struct folio *folio)    \
{                                    \
    if (folio_test_##fname(folio))                    \
        return;                            \
    VM_BUG_ON_FOLIO(data_race(folio->page.page_type) != UINT_MAX,    \
            folio);                        \
    folio->page.page_type = (unsigned int)PGTY_##lname << 24;    \
}                                    \
static __always_inline void __folio_clear_##fname(struct folio *folio)    \
{                                    \
    if (folio->page.page_type == UINT_MAX)                \
        return;                            \
    VM_BUG_ON_FOLIO(!folio_test_##fname(folio), folio);        \
    folio->page.page_type = UINT_MAX;                \
}
```

```
allocate_slab
alloc_slab_page
alloc_frozen_pages_noprof mempolicy.c里面的

slab_address这个展开后的
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) void *lowmem_page_address(const struct page *page)
{
 return ((void *)((unsigned long)(((phys_addr_t)((unsigned long)((page) - ((struct page *)vmemmap_base))) << 12))+((unsigned long)page_offset_base)));
}
```

```
__pa(x)的宏展开
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) unsigned long __phys_addr_nodebug(unsigned long x)
{
    unsigned long y = x - (0xffffffff80000000UL);

     x = y + ((x > y) ? phys_base : ((0xffffffff80000000UL) - ((unsigned long)page_offset_base)));

     return x;
}
```

最小收集page

[__rmqueue_smallest](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__rmqueue_smallest)
