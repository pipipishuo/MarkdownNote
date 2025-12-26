# 初始化调试

第一句断点

```
b *0x21fb6d0
```

在第一句时就已经进入了保护模式和开启了分页



根据这个推出的第一句解压完的内核代码

```
(gdb) b startup_64
Breakpoint 1 at 0xffffffff821fb6d0: file arch/x86/kernel/head_64.S, line 59.
```

查看反汇编

```
x /10i 0x21fb6d0
```



到那句push断点就不管事了 用这个接上

```
b *0x21fb6f4
```



这是构建表call __startup_64那块

```
b *0x21fb6fb
```





0x00000000021fb71e   这个地址是个突变

0x21fb71b



secondary_startup_64的断点

```
b *0xffffffff812993d8
```



# __startup_64

构建的是2M的页映射  不是4KB 我擦了我  倒是说一句啊你  我说怎么对不上4KB一页呢

主要是这里

```
pmd_entry = __PAGE_KERNEL_LARGE_EXEC & ~_PAGE_GLOBAL;
展开
pmd_entry = ((((pteval_t)(1)) << 0)|(((pteval_t)(1)) << 1)| 0|(((pteval_t)(1)) << 5)| 0|(((pteval_t)(1)) << 6)|(((pteval_t)(1)) << 7)|(((pteval_t)(1)) << 8)) & ~(((pteval_t)(1)) << 8);
```

开发手册上写着呢

```
Use of the PDE depends on its PS flag:
• If the PDE's PS flag is 1, the PDE maps a 2-MByte page (see Table 5-18). The final physical address is computed 
as follows:
— Bits 51:21 are from the PDE.
— Bits 20:0 are from the original linear address.
The linear address’s protection key is the value of bits 62:59 of the PDE (see Section 5.6.2).
• If the PDE’s PS flag is 0, a 4-KByte naturally aligned page table is located at the physical address specified in 
bits 51:12 of the PDE (see Table 5-19). A page table comprises 512 64-bit entries (PTEs). A PTE is selected 
using the physical address defined as follows:
— Bits 51:12 are from the PDE.
— Bits 11:3 are bits 20:12 of the linear address.
— Bits 2:0 are all 0.
```

ps flag是第7个bit值  如果这个为1 那就是2M 分页  

注意！ ps flag 是第7个  噢噢噢噢  这样就对上了

第一个是P flag 不是PS flag
