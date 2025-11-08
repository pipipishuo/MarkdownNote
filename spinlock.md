一般就用这个lock

参考链接 [spinlock_types.h - include/linux/spinlock_types.h - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/include/linux/spinlock_types.h#L29)

# spin_lock

参考链接 [spinlock.h - include/linux/spinlock.h - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/include/linux/spinlock.h#L349)

```
static __always_inline void spin_lock(spinlock_t *lock)
{
    raw_spin_lock(&lock->rlock);
}

#define raw_spin_lock(lock)    _raw_spin_lock(lock)
```

就是一个封装

# _raw_spin_lock

这个就分为两个路子了一个up的 一个smp的

先看up的

# up版

话说up指什么？参考链接[Linux中的SMP与UP_smp和up-CSDN博客](https://blog.csdn.net/Erice_s/article/details/118284087)  说白了就是一个cpu 也就是单核 哦哦 那就对了 这也就是底下只需要阻止抢断就行了

```
#define _raw_spin_lock(lock)            __LOCK(lock)
```

```
#define __LOCK(lock) \
  do { preempt_disable(); ___LOCK(lock); } while (0)
```

```
#define ___LOCK(lock) \
  do { __acquire(lock); (void)(lock); } while (0)
```

这又到另外一个领域了 preempt 挺好  我倒要看看有多少领域

```
#define preempt_disable() \
do { \
    preempt_count_inc(); \
    barrier(); \
} while (0)
```

```
#define preempt_count_inc() preempt_count_add(1)
```

```
#define preempt_count_add(val)    __preempt_count_add(val)
```

```
static __always_inline void __preempt_count_add(int val)
{
    raw_cpu_add_4(__preempt_count, val);
}
```

```
DECLARE_PER_CPU_CACHE_HOT(int, __preempt_count);
```

```
#define DECLARE_PER_CPU_CACHE_HOT(type, name)                \
    DECLARE_PER_CPU_SECTION(type, name, "..hot.." #name)
```

```
#define DECLARE_PER_CPU_SECTION(type, name, sec)            \
    extern __PCPU_ATTRS(sec) __typeof__(type) name
```

```
#define __PCPU_ATTRS(sec)                        \
    __percpu __attribute__((section(PER_CPU_BASE_SECTION sec)))    \
    PER_CPU_ATTRIBUTES
```

```
# define __percpu    __percpu_qual BTF_TYPE_TAG(percpu)
```

```
# define __percpu_qual        __percpu_seg_override
```

```
#define __percpu_seg_override    CONCATENATE(__seg_, __percpu_seg)
```

```
#define __CONCAT(a, b) a ## b
#define CONCATENATE(a, b) __CONCAT(a, b)
```

```
# define __percpu_seg        gs
```

```
__percpu_qual  __seg_gs
```

```
# define BTF_TYPE_TAG(value) __attribute__((btf_type_tag(#value)))
```

btf_type_tag参考链接[Common Type Attributes (Using the GNU Compiler Collection (GCC))](https://gcc.gnu.org/onlinedocs/gcc/Common-Type-Attributes.html) 

综上

```
# define __percpu    __percpu_qual BTF_TYPE_TAG(percpu)
__percpu    __seg_gs __attribute__((btf_type_tag(percpu)))
```

```
# definePER_CPU_ATTRIBUTES
```

```
#define PER_CPU_BASE_SECTION ".data..percpu"
```

```
//所以__PCPU_ATTRS 原本长这样
#define __PCPU_ATTRS(sec)                        \
    __seg_gs __attribute__((btf_type_tag(percpu))) __attribute__((section(".data..percpu" sec)))
```

```
#define DECLARE_PER_CPU_SECTION(type, name, sec)            \
    extern__seg_gs __attribute__((btf_type_tag(percpu))) __attribute__((section(".data..percpu" sec)))) __typeof__(type) name
```

```
#define DECLARE_PER_CPU_CACHE_HOT(type, name)
DECLARE_PER_CPU_SECTION(type, name, "..hot.." #name)


DECLARE_PER_CPU_CACHE_HOT(int, __preempt_count);
DECLARE_PER_CPU_SECTION(int,__preempt_count,"..hot..__preempt_count")
```

最后展开就是这个

```
extern __seg_gs __attribute__((btf_type_tag(percpu))) __attribute__((section(".data..percpu..hot..__preempt_count" )))) __typeof__(type) name
```

在.data..percpu..hot..__preempt_count数据段中 存着一个叫__preempt_count的整型数值 

关于percpu 参考 [(21 封私信 / 9 条消息) 一张图看懂linux内核中percpu变量的实现 - 知乎](https://zhuanlan.zhihu.com/p/340985476)

其中有句话说的挺好

linux内核在启动时，会先把vmlinux文件加载到内存中，然后根据cpu的个数，为每个cpu都分配一块用于存放percpu变量的内存区域，之后把vmlinux中的.data..percpu section里的内容，拷贝到各个cpu的percpu内存块的static区域里，最后将各percpu内存块的起始地址放到对应cpu的[gs寄存器](https://zhida.zhihu.com/search?content_id=163904224&content_type=Article&match_order=1&q=gs%E5%AF%84%E5%AD%98%E5%99%A8&zhida_source=entity)里。

那运行时是怎么样的呢？

__seg_gs就起作用了  这个是更改了变量的访问路径  之前可能是栈 这个直接变成了从gs提供的数据段中访问  这样就解释的通了

__seg_gs的参考链接[Named Address Spaces (Using the GNU Compiler Collection (GCC))](https://gcc.gnu.org/onlinedocs/gcc/Named-Address-Spaces.html)

好啦 回到开头，刚才说的都是关于__preempt_count这个变量的事儿，原本要展开的代码是这个

```
static __always_inline void __preempt_count_add(int val)
{
    raw_cpu_add_4(__preempt_count, val);
}
```

使用宏展开方法 代码如下

```
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) void __preempt_count_add(int val)
{
    do
    {
        const int pao_ID__ = (__builtin_constant_p(val) && ((val) == 1 || (val) == (typeof(val)) - 1)) ? (int)(val) : 0;
        if (0) {
            __typeof_unqual__((__preempt_count)) pao_tmp__;
            pao_tmp__ = (val); 
            (void)pao_tmp__;
        }
        if (pao_ID__ == 1) 
            ({ asm("inc" "l " "%" "[var]" : [var] "+m" (((__preempt_count)))); });
        else if (pao_ID__ == -1) 
            ({ asm("dec" "l " "%" "[var]" : [var] "+m" (((__preempt_count)))); }); 
        else 
            do { 
                u32 pto_val__ = ((u32)(((unsigned long)val) & 0xffffffff)); 
                if (0) { 
                    __typeof_unqual__((__preempt_count)) pto_tmp__; 
                    pto_tmp__ = (val); (void)pto_tmp__; 
                } 
                asm("add" "l " "%[val], " "%" "[var]" : [var] "+m" (((__preempt_count))) : [val] "ri" (pto_val__)); 
            } while (0);
    }while (0);
}
```

根据这个也就能看出来为啥preempt_disable宏不加锁了  因为核心执行的就是一条汇编语句 执行汇编语句的时候是不会被中断的

接下来该barrier()了

```
# define barrier() __asm__ __volatile__("": : :"memory")
```

意思就是别让编译器优化时排列内存访问顺序  对 如果把前后的__preempt_count访问代码优化后都放一起  那有可能出现失效 

参考这个链接[c - Working of __asm__ __volatile__ ("" : : : "memory") - Stack Overflow](https://stackoverflow.com/questions/14950614/working-of-asm-volatile-memory)

# smp版

```
__attribute__((__noinline__)) void __attribute__((__section__(".spinlock.text"))) _raw_spin_lock(raw_spinlock_t *lock)
{
 __raw_spin_lock(lock);
}
```

这个参照spinlock.i文件（也就是spinlock.c展开后的文件）

```
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) void _
_raw_spin_lock(raw_spinlock_t *lock)
{
 do { __preempt_count_add(1); __asm__ __volatile__("": : :"memory"); } while (0);
 do { } while (0);
 do_raw_spin_lock(lock); 
} 
```

```
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) void d
o_raw_spin_lock(raw_spinlock_t *lock)
{
 (void)0;
 queued_spin_lock(&lock->raw_lock);
 do { } while (0);
}
```

```
static __always_inline void queued_spin_lock(struct qspinlock *lock)
{
    int val = 0;

    if (likely(atomic_try_cmpxchg_acquire(&lock->val, &val, _Q_LOCKED_VAL)))
        return;

    queued_spin_lock_slowpath(lock, val);
}
```

[queued_spin_lock_slowpath](https://elixir.bootlin.com/linux/v6.16.3/C/ident/queued_spin_lock_slowpath) 就看源码吧 也能用源码目录下的qspinlock.i作参考 这个是宏展开后的

```
 static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) bool atomic_try_cmpxchg_acquire(atomic_t *v, int *old, int new)
{
 instrument_atomic_read_write(v, sizeof(*v));
 instrument_atomic_read_write(old, sizeof(*old));
 return raw_atomic_try_cmpxchg_acquire(v, old, new);
}

raw_atomic_try_cmpxchg_acquire
arch_atomic_try_cmpxchg
```

arch_atomic_try_cmpxchg展开后

```
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) bool arch_atomic_try_cmpxchg(atomic_t* v, int* old, int new)
{
     return ({ 
        bool success;
         __typeof__(((&v->counter))) _old = (__typeof__(((&v->counter))))(((old)));
         __typeof__(*(((&v->counter)))) __old = *_old;
         __typeof__(*(((&v->counter)))) __new = (((new)));
         switch ((sizeof(*(&v->counter)))) { 
             case 1: { 
                 volatile u8* __ptr = (volatile u8*)(((&v->counter))); 
                 asm __inline volatile(".pushsection .smp_locks,\"a\"\n" ".balign 4\n" ".long 671f - .\n" ".popsection\n" "671:" "\n\tlock " "cmpxchgb %[new], %[ptr]" "\n\t/* output condition code " "z" "*/\n" : "=@cc" "z" (success),[ptr] "+m" (*__ptr),[old] "+a" (__old) : [new] "q" (__new) : "memory");
                break; 
             } 
             case 2: { 
                 volatile u16* __ptr = (volatile u16*)(((&v->counter)));
                 asm __inline volatile(".pushsection .smp_locks,\"a\"\n" ".balign 4\n" ".long 671f - .\n" ".popsection\n" "671:" "\n\tlock " "cmpxchgw %[new], %[ptr]" "\n\t/* output condition code " "z" "*/\n" : "=@cc" "z" (success),[ptr] "+m" (*__ptr),[old] "+a" (__old) : [new] "r" (__new) : "memory");
                 break; 
             } 
             case 4: { 
                 volatile u32* __ptr = (volatile u32*)(((&v->counter))); 
                 asm __inline volatile(".pushsection .smp_locks,\"a\"\n" ".balign 4\n" ".long 671f - .\n" ".popsection\n" "671:" "\n\tlock " "cmpxchgl %[new], %[ptr]" "\n\t/* output condition code " "z" "*/\n" : "=@cc" "z" (success),[ptr] "+m" (*__ptr),[old] "+a" (__old) : [new] "r" (__new) : "memory");
                break; 
             } 
             case 8: { 
                 volatile u64* __ptr = (volatile u64*)(((&v->counter))); 
                 asm __inline volatile(".pushsection .smp_locks,\"a\"\n" ".balign 4\n" ".long 671f - .\n" ".popsection\n" "671:" "\n\tlock " "cmpxchgq %[new], %[ptr]" "\n\t/* output condition code " "z" "*/\n" : "=@cc" "z" (success),[ptr] "+m" (*__ptr),[old] "+a" (__old) : [new] "r" (__new) : "memory"); 
                break; 
             } 
             default: 
                 __cmpxchg_wrong_size(); 
         } 
         if (__builtin_expect(!!(!success), 0)) *_old = __old; __builtin_expect(!!(success), 1); 
     });
}
```
