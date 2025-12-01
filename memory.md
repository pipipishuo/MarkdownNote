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

最小收集page调用栈

[__rmqueue_smallest](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__rmqueue_smallest)

[__rmqueue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__rmqueue)

[rmqueue_buddy](https://elixir.bootlin.com/linux/v6.16.3/C/ident/rmqueue_buddy)

[rmqueue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/rmqueue)

[get_page_from_freelist](https://elixir.bootlin.com/linux/v6.16.3/C/ident/get_page_from_freelist)

[__alloc_frozen_pages_noprof](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__alloc_frozen_pages_noprof)

[alloc_slab_page](https://elixir.bootlin.com/linux/v6.16.3/C/ident/alloc_slab_page)

[allocate_slab](https://elixir.bootlin.com/linux/v6.16.3/C/ident/allocate_slab)

[mem_map](https://elixir.bootlin.com/linux/v6.16.3/C/ident/mem_map)  存放struct page的数组        //但默认开启的是NUMA  不开启NUMA才会用这个

[__init_single_page](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__init_single_page)

[memmap_init_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/memmap_init_range)

[memmap_init_zone_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/memmap_init_zone_range)  关于zone大小确定的

[memmap_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/memmap_init)

[free_area_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/free_area_init)

[zone_sizes_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/zone_sizes_init)

构建pgt表

[early_make_pgtable](https://elixir.bootlin.com/linux/v6.16.3/C/ident/early_make_pgtable)

这个应该是最后的切换吧

[head_64.S - arch/x86/kernel/head_64.S - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/arch/x86/kernel/head_64.S#L183)

64位线性地址转为物理地址流程:

intel 开发手册卷三  5.5.3 Use of HLATP with HLAT 4-Level Paging and 5-Level Paging

**最大物理地址位数**   **MAXPHYADDR**  很重要的概念  md 线性地址能够到64 物理地址到不了  够艹的

内存管理模型顺序是啥  反正肯定得依赖x86分页

是这样的吗？

1 实模式

2 64位未开分页模式

3 64位开分页模式

*setup_initial_init_mm*  初始化init_mm变量的

init_mm 挺关键的  她应该是内存管理的核心变量  第一个

[load_new_mm_cr3](https://elixir.bootlin.com/linux/v6.16.3/C/ident/load_new_mm_cr3)导入cr3的函数  也挺核心的

[dup_mmap](https://elixir.bootlin.com/linux/v6.16.3/source/mm/mmap.c#L1723)

[__pte_alloc](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__pte_alloc)  收集pte用的  这是我今天想找的

[set_ptes](https://elixir.bootlin.com/linux/v6.16.3/C/ident/set_ptes)  这应该就是安pte目录

[__pmd_alloc](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__pmd_alloc) 这个不太一样

[preallocate_vmalloc_pages](https://elixir.bootlin.com/linux/v6.16.3/C/ident/preallocate_vmalloc_pages)

[walk_to_pmd](https://elixir.bootlin.com/linux/v6.16.3/C/ident/walk_to_pmd)  应该挺有用的  应该揭示了pgd pud pmd 怎么找的

[pmd_offset](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pmd_offset)  很好的案例怎么根据pud找到pmd的

# 如何分配page  V6.16版本

从小往大找 找到最近的那个大的

然后取出要用的那部分，然后把大的分解成小的  [__rmqueue_smallest](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__rmqueue_smallest)

如  order=4  最近的order是16  取出第一个order为4的 把剩下的 拆解成order=4,5,6,7,8........15 放到各自里面

为什么可以这样拆  因为

$$
2^{16}=2^{15}+2^{14}+.....+2^{4}+2^{4}
$$

前面那一堆再加一次2的4次方即可  牛逼！

# early_top_pgt ，init_top_pgt ，init_mm->pgd 三者之间的关系

根据我现在的观察 

early_top_pgt 是内核早起构造的 构造完把第511项个给init_top_pgt 这样两者都在fff那指向同一块物理内存了（因为511的二进制就全是1）

init_mm->pgd的值是跟init_top_pgt一样的 这样就都能对上了

# pdg p4d pud pmd pte 都怎么存的

也是按页存！想不到吧

就是先分配个页  然后把数据写到这个页中  分配页的方式跟分配普通的页的方式一模一样  这就存在一个问题  怎么能保证在写入页表项时不至于报缺页异常呢？也就是虚拟地址能转为物理地址，按照现在观察 我觉得是init_mm->pgd 已经安排好了这部分 所以不怕它缺页 要不然说不通



# 探索物理内存大小

[memblock_add_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/memblock_add_range)

[e820__memblock_setup](https://elixir.bootlin.com/linux/v6.16.3/C/ident/e820__memblock_setup)  这个把e820和memblock.memory连接到一起的  这个很关键

主要变量 [e820_table](https://elixir.bootlin.com/linux/v6.16.3/C/ident/e820_table) 



[detect_memory_e820](https://elixir.bootlin.com/linux/v6.16.3/C/ident/detect_memory_e820)  应该是用了  在启动的时候但vmlinux中没有查到

[e820__memory_setup_default](https://elixir.bootlin.com/linux/v6.16.3/C/ident/e820__memory_setup_default)

[e820__memory_setup](https://elixir.bootlin.com/linux/v6.16.3/C/ident/e820__memory_setup)



# zone大小确定



[memmap_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/memmap_init)关于zone大小确定的

[__next_mem_pfn_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__next_mem_pfn_range) 这个是把memblock.memory与zone连接到一起的  算出out_start_pfn和out_end_pfn 然后赋值给zone



# node初始化

[alloc_node_data](https://elixir.bootlin.com/linux/v6.16.3/C/ident/alloc_node_data) node初始化

[__next_mem_range_rev](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__next_mem_range_rev)  这个里面解答了通过memblock.memory和memblock.reservered分配的node地址



# 从e820到zone的流程

e820__memblock_setup  这个把e820和memblock.memory连接到一起的  这个很关键

initmem_init 里面的alloc_node_data初始化node

zone_sizes_init （在setup_arch中是x86_init.paging.pagetable_init）里的 memmap_init （其实是这个[__next_mem_pfn_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__next_mem_pfn_range)）确定了 将memblock.memory与zone连接到一起的  算出out_start_pfn和out_end_pfn 然后赋值给zone 然后放到node中

# memblock变量

定义处：mm / memblock.c  128  v6.16

```
struct memblock memblock __initdata_memblock = {
	.memory.regions		= memblock_memory_init_regions,
	.memory.max		= INIT_MEMBLOCK_MEMORY_REGIONS,
	.memory.name		= "memory",

	.reserved.regions	= memblock_reserved_init_regions,
	.reserved.max		= INIT_MEMBLOCK_RESERVED_REGIONS,
	.reserved.name		= "reserved",

	.bottom_up		= false,
	.current_limit		= MEMBLOCK_ALLOC_ANYWHERE,
};

```

关于memblock.reserved是从哪开始加的 [__memblock_reserve](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__memblock_reserve)   断点打这就知道了



# 缺页中断

[finish_fault](https://elixir.bootlin.com/linux/v6.16.3/C/ident/finish_fault)  这是第一个入手点

[set_ptes](https://elixir.bootlin.com/linux/v6.16.3/C/ident/set_ptes)  这也是  准备设置页了都



# 存page数据的物理地址是放哪的

线性地址是这儿 

```
ffffea0000000000 |  -22    TB | ffffeaffffffffff |    1 TB | virtual memory map (vmemmap_base)
```

## SPARSEMEM

参考链接https://www.kernel.org/doc/html/latest/mm/memory-model.html#sparsemem 

这个应该是我要的  

```
The sparse vmemmap uses a virtually mapped memory map to optimize pfn_to_page and page_to_pfn operations. There is a global struct page *vmemmap pointer that points to a virtually contiguous array of struct page objects. A PFN is an index to that array and the offset of the struct page from vmemmap is the PFN of that page.

To use vmemmap, an architecture has to reserve a range of virtual addresses that will map the physical pages containing the memory map and make sure that vmemmap points to that range. In addition, the architecture should implement vmemmap_populate() method that will allocate the physical memory and create page tables for the virtual memory map. If an architecture does not have any special requirements for the vmemmap mappings, it can use default vmemmap_populate_basepages() provided by the generic memory management.
```

[vmemmap_populate](https://elixir.bootlin.com/linux/v6.16.3/C/ident/vmemmap_populate)  关键函数

SPARSEMEM是按128M为一段映射的 内存如果是512M就执行四次vmemmap_populate

[memory_present](https://elixir.bootlin.com/linux/v6.16.3/C/ident/memory_present)  从这分的段 为啥要分段啊  哦！因为在[vmemmap_set_pmd](https://elixir.bootlin.com/linux/v6.16.3/C/ident/vmemmap_set_pmd) 中看到制作的pte就是2M分页去做的  这样说内核都是2M分页了？  128M的内存正好需要2M的page数组去存 
$$
32768*64=2*1024*1024
$$
[__populate_section_memmap](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__populate_section_memmap)  将页值转为线性地址

```
unsigned long start = (unsigned long) pfn_to_page(pfn);
```



[vmemmap_alloc_block_buf](https://elixir.bootlin.com/linux/v6.16.3/C/ident/vmemmap_alloc_block_buf) 这个是收集物理地址的函数返回的是个线性地址 第一次是这个位置0xffff88801f600000 看着很眼熟   我猜应该紧随代码后面



[vmemmap_set_pmd](https://elixir.bootlin.com/linux/v6.16.3/C/ident/vmemmap_set_pmd) 这里面把上面收集到的线性地址转为物理地址并做成了pmd页

```
pte_t entry;

	entry = pfn_pte(__pa(p) >> PAGE_SHIFT,
			PAGE_KERNEL_LARGE);
```

[sparse_buffer_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sparse_buffer_init)  

sparsemap_buf  存放page数组的基地址变量

这个数组也是分过来的 好怪啊感觉

哦不怪 [__next_mem_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__next_mem_range) 是通过这个直接分的  没走那个[__rmqueue_smallest](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__rmqueue_smallest)的分配路子  我说呢 如果这个再走这个路子  那不死循环了?
