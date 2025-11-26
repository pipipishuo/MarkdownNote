# 初始化调试

第一句断点

```
b *0x21fb6d0
```

根据这个推出的第一句解压完的内核代码

```
(gdb) b startup_64
Breakpoint 1 at 0xffffffff821fb6d0: file arch/x86/kernel/head_64.S, line 59.
```

查看反汇编

```
x /10i 0x21fb6d0
```



```
b *0x21fb6f4
```





0x00000000021fb71e   这个地址是个突变

0x21fb71b



secondary_startup_64的断点

```
b *0xffffffff812993d8
```
