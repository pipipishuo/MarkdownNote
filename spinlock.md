一般就用这个lock

参考链接 [spinlock_types.h - include/linux/spinlock_types.h - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/include/linux/spinlock_types.h#L29)

# spin_lock

## spin_lock

参考链接 [spinlock.h - include/linux/spinlock.h - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/include/linux/spinlock.h#L349)

```
static __always_inline void spin_lock(spinlock_t *lock)
{
    raw_spin_lock(&lock->rlock);
}

#define raw_spin_lock(lock)    _raw_spin_lock(lock)
```

就是一个封装

## _raw_spin_lock

这个就分为两个路子了一个up的 一个smp的

先看up的

## up版

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

## smp版

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

以case2为例 把其余的去掉 核心语句就是这个

```
lock cmpxchgq  %[new], %[ptr]
```

关于lock 前缀  看开发手册  LOCK—Assert LOCK# Signal Prefix

就是说伴随的指令是只能被它使用

```
Causes the processor’s LOCK# signal to be asserted during execution of the accompanying instruction (turns the 
instruction into an atomic instruction). In a multiprocessor environment, the LOCK# signal ensures that the 
processor has exclusive use of any shared memory while the signal is asserted.
```





[queued_spin_lock_slowpath](https://elixir.bootlin.com/linux/v6.16.3/C/ident/queued_spin_lock_slowpath) 这个是真正符合我对自旋锁定义的代码 也是自旋锁的核心

一些重要函数（宏展开版）

```
void __preempt_count_add(int val)
{
    do { 
        const int pao_ID__ = (__builtin_constant_p(val) && ((val) == 1 || (val) == (typeof(val)) - 1)) ? (int)(val) : 0; 
        if (0) { 
            __typeof_unqual__((__preempt_count)) pao_tmp__; pao_tmp__ = (val); (void)pao_tmp__; 
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
                    pto_tmp__ = (val); 
                    (void)pto_tmp__; 
                } 
                asm("add" "l " "%[val], " "%" "[var]" : [var] "+m" (((__preempt_count))) : [val] "ri" (pto_val__)); 
            } while (0); 
    } while (0);
}
static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) 
bool arch_atomic_try_cmpxchg(atomic_t* v, int* old, int new)
{
 return (
     { bool success; 
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
     volatile u64* __ptr = (volatile u64*)(((&v->counter))); asm __inline volatile(".pushsection .smp_locks,\"a\"\n" ".balign 4\n" ".long 671f - .\n" ".popsection\n" "671:" "\n\tlock " "cmpxchgq %[new], %[ptr]" "\n\t/* output condition code " "z" "*/\n" : "=@cc" "z" (success),[ptr] "+m" (*__ptr),[old] "+a" (__old) : [new] "r" (__new) : "memory"); 
     break; 
 } 
 default:
     __cmpxchg_wrong_size(); 
 } 
 if (__builtin_expect(!!(!success), 0)) *_old = __old; 
 __builtin_expect(!!(success), 1);
  });
}

static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) int arch_atomic_read(const atomic_t* v)
{


    return (*(const volatile typeof(_Generic(((v)->counter), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: ((v)->counter)))*)& ((v)->counter));
}

{
    if (val & (((1U << 8) - 1) << 0))
        ({ 
     typeof(_Generic((*&lock->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&lock->locked))) _val; 
    _val = ({ typeof(&lock->locked) __PTR = (&lock->locked);
    typeof(_Generic((*&lock->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&lock->locked))) VAL; 
    for (;;) { 
        VAL = (
            { 
                do { 
                    __attribute__((__noreturn__)) extern void __compiletime_assert_445(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); 
                    if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) 
                        __compiletime_assert_445(); 
                } while (0); 
        (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); 
            }
            ); 
        if (!VAL) break; 
        cpu_relax(); 
    } 
    (typeof(*&lock->locked))VAL; }); 
    do { 
        do {} while (0);
        do { 
            do {} while (0);
            __asm__ __volatile__("": : : "memory"); 
        } while (0); 
    } while (0); 
    (typeof(*&lock->locked))_val; });
}

static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) void clear_pending_set_locked(struct qspinlock* lock)
{
    do { 
        do {
            __attribute__((__noreturn__)) extern void __compiletime_assert_435(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); 
            if (!((sizeof(lock->locked_pending) == sizeof(char) || sizeof(lock->locked_pending) == sizeof(short) || sizeof(lock->locked_pending) == sizeof(int) || sizeof(lock->locked_pending) == sizeof(long)) || sizeof(lock->locked_pending) == sizeof(long long))) __compiletime_assert_435(); 
        } while (0); 
        do { 
            *(volatile typeof(lock->locked_pending)*)& (lock->locked_pending) = ((1U << 0)); 
        } while (0); 
    } while (0);
}

void __attribute__((__section__(".spinlock.text"))) queued_spin_lock_slowpath(struct qspinlock* lock, u32 val)
{
    struct mcs_spinlock* prev, * next, * node;
    u32 old, tail;
    int idx;

    do { __attribute__((__noreturn__)) extern void __compiletime_assert_443(void) __attribute__((__error__("BUILD_BUG_ON failed: " "CONFIG_NR_CPUS >= (1U << _Q_TAIL_CPU_BITS)"))); if (!(!(64 >= (1U << (32 - (((0 + 8) + 8) + 2)))))) __compiletime_assert_443(); } while (0);

    if (false)
        goto pv_queue;

    if (virt_spin_lock(lock))
        return;







    if (val == (1U << (0 + 8))) {
        int cnt = (1 << 9);
        val = ({ 
            typeof(&(&lock->val)->counter) __PTR = (&(&lock->val)->counter); typeof(_Generic((*&(&lock->val)->counter), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&(&lock->val)->counter))) VAL;
        for (;;) { 
            VAL = ({ 
                do {
                    __attribute__((__noreturn__)) extern void __compiletime_assert_444(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); 
                    if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) __compiletime_assert_444(); 
                } while (0); 
            (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); 
                }); 
            if (((VAL != (1U << (0 + 8))) || !cnt--)) break; 
            cpu_relax(); 
        } 
        (typeof(*&(&lock->val)->counter))VAL; });
    }




    if (val & ~(((1U << 8) - 1) << 0))
        goto queue;






    val = queued_fetch_set_pending_acquire(lock);
    # 176 "kernel/locking/qspinlock.c"
        if (__builtin_expect(!!(val & ~(((1U << 8) - 1) << 0)), 0)) {


            if (!(val & (((1U << 8) - 1) << (0 + 8))))
                clear_pending(lock);

            goto queue;
        }
    # 196 "kernel/locking/qspinlock.c"
        if (val & (((1U << 8) - 1) << 0))
            ({ 
            typeof(_Generic((*&lock->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&lock->locked))) _val;
            _val = ({ typeof(&lock->locked) __PTR = (&lock->locked); 
                      typeof(_Generic((*&lock->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&lock->locked))) VAL; 
                      for (;;) { 
                          VAL = ({ 
                              do { 
                                  __attribute__((__noreturn__)) extern void __compiletime_assert_445(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); 
                                  if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) __compiletime_assert_445(); 
                              } while (0); 
                          (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); 
                              }); 
                          if (!VAL) break;
                          cpu_relax(); 
                      } 
                      (typeof(*&lock->locked))VAL; }); 
            do { 
                do {
                } while (0); 
                do { 
                    do {
                    } while (0); 
                    __asm__ __volatile__("": : : "memory"); 
                } while (0); 
            } while (0); 
            (typeof(*&lock->locked))_val; });






    clear_pending_set_locked(lock);
    ;
    return;





queue:
    ;
pv_queue:
    node = ({ do { const void __seg_gs* __vpp_verify = (typeof((&qnodes[0].mcs) + 0))((void*)0); (void)__vpp_verify; } while (0); ({ unsigned long tcp_ptr__ = ({ *(typeof(this_cpu_off)*)(&(this_cpu_off)); }); tcp_ptr__ += (unsigned long)(&qnodes[0].mcs); (__typeof_unqual__(*(&qnodes[0].mcs))*)tcp_ptr__; }); });
    idx = node->count++;
 tail = encode_tail(({ __this_cpu_preempt_check("read"); ({ __typeof_unqual__(cpu_number) pscr_ret__; do { const void __seg_gs* __vpp_verify = (typeof((&(cpu_number)) + 0))((void*)0); (void)__vpp_verify; } while (0); switch (sizeof(cpu_number)) { case 1: pscr_ret__ = ({ *(typeof(cpu_number)*)(&(cpu_number)); }); break; case 2: pscr_ret__ = ({ *(typeof(cpu_number)*)(&(cpu_number)); }); break; case 4: pscr_ret__ = ({ *(typeof(cpu_number)*)(&(cpu_number)); }); break; case 8: pscr_ret__ = ({ *(typeof(cpu_number)*)(&(cpu_number)); }); break; default: __bad_size_call_parameter(); break; } pscr_ret__; }); }), idx);

                                                                                                                                                                                                                                                             trace_contention_begin(lock, (1U << 0));
                                                                                                                                                                                                                                                             # 230 "kernel/locking/qspinlock.c"
                                                                                                                                                                                                                                                                 if (__builtin_expect(!!(idx >= 4), 0)) {
                                                                                                                                                                                                                                                                     ;
                                                                                                                                                                                                                                                                     while (!queued_spin_trylock(lock))
                                                                                                                                                                                                                                                                         cpu_relax();
                                                                                                                                                                                                                                                                     goto release;
                                                                                                                                                                                                                                                                 }

                                                                                                                                                                                                                                                             node = grab_mcs_node(node, idx);




                                                                                                                                                                                                                                                             do { (void)(idx); } while (0);






                                                                                                                                                                                                                                                             __asm__ __volatile__("": : : "memory");

                                                                                                                                                                                                                                                             node->locked = 0;
                                                                                                                                                                                                                                                             node->next = ((void*)0);
                                                                                                                                                                                                                                                             __pv_init_node(node);






                                                                                                                                                                                                                                                             if (queued_spin_trylock(lock))
                                                                                                                                                                                                                                                                 goto release;






                                                                                                                                                                                                                                                             do { do {} while (0); __asm__ __volatile__("": : : "memory"); } while (0);
                                                                                                                                                                                                                                                             # 277 "kernel/locking/qspinlock.c"
                                                                                                                                                                                                                                                                 old = xchg_tail(lock, tail);
                                                                                                                                                                                                                                                             next = ((void*)0);





                                                                                                                                                                                                                                                             if (old & ((((1U << 2) - 1) << ((0 + 8) + 8)) | (((1U << (32 - (((0 + 8) + 8) + 2))) - 1) << (((0 + 8) + 8) + 2)))) {
                                                                                                                                                                                                                                                                 prev = decode_tail(old, qnodes);


                                                                                                                                                                                                                                                                 do { do { __attribute__((__noreturn__)) extern void __compiletime_assert_446(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); if (!((sizeof(prev->next) == sizeof(char) || sizeof(prev->next) == sizeof(short) || sizeof(prev->next) == sizeof(int) || sizeof(prev->next) == sizeof(long)) || sizeof(prev->next) == sizeof(long long))) __compiletime_assert_446(); } while (0); do { *(volatile typeof(prev->next)*)& (prev->next) = (node); } while (0); } while (0);

                                                                                                                                                                                                                                                                 __pv_wait_node(node, prev);
                                                                                                                                                                                                                                                                 ({ typeof(_Generic((*&node->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&node->locked))) _val; _val = ({ typeof(&node->locked) __PTR = (&node->locked); typeof(_Generic((*&node->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&node->locked))) VAL; for (;;) { VAL = ({ do { __attribute__((__noreturn__)) extern void __compiletime_assert_447(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) __compiletime_assert_447(); } while (0); (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); }); if (VAL) break; cpu_relax(); } (typeof(*&node->locked))VAL; }); do { do {} while (0); do { do {} while (0); __asm__ __volatile__("": : : "memory"); } while (0); } while (0); (typeof(*&node->locked))_val; });







                                                                                                                                                                                                                                                                 next = ({ do { __attribute__((__noreturn__)) extern void __compiletime_assert_448(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); if (!((sizeof(node->next) == sizeof(char) || sizeof(node->next) == sizeof(short) || sizeof(node->next) == sizeof(int) || sizeof(node->next) == sizeof(long)) || sizeof(node->next) == sizeof(long long))) __compiletime_assert_448(); } while (0); (*(const volatile typeof(_Generic((node->next), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (node->next)))*)& (node->next)); });
                                                                                                                                                                                                                                                                 if (next)
                                                                                                                                                                                                                                                                     prefetchw(next);
                                                                                                                                                                                                                                                             }
                                                                                                                                                                                                                                                             # 325 "kernel/locking/qspinlock.c"
                                                                                                                                                                                                                                                                 if ((val = __pv_wait_head_or_lock(lock, node)))
                                                                                                                                                                                                                                                                     goto locked;

                                                                                                                                                                                                                                                             val = ({ typeof(_Generic((*&(&lock->val)->counter), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&(&lock->val)->counter))) _val; _val = ({ typeof(&(&lock->val)->counter) __PTR = (&(&lock->val)->counter); typeof(_Generic((*&(&lock->val)->counter), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&(&lock->val)->counter))) VAL; for (;;) { VAL = ({ do { __attribute__((__noreturn__)) extern void __compiletime_assert_449(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) __compiletime_assert_449(); } while (0); (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); }); if ((!(VAL & ((((1U << 8) - 1) << 0) | (((1U << 8) - 1) << (0 + 8)))))) break; cpu_relax(); } (typeof(*&(&lock->val)->counter))VAL; }); do { do {} while (0); do { do {} while (0); __asm__ __volatile__("": : : "memory"); } while (0); } while (0); (typeof(*&(&lock->val)->counter))_val; });

                                                                                                                                                                                                                                                         locked:
                                                                                                                                                                                                                                                             # 352 "kernel/locking/qspinlock.c"
                                                                                                                                                                                                                                                                 if ((val & ((((1U << 2) - 1) << ((0 + 8) + 8)) | (((1U << (32 - (((0 + 8) + 8) + 2))) - 1) << (((0 + 8) + 8) + 2)))) == tail) {
                                                                                                                                                                                                                                                                     if (atomic_try_cmpxchg_relaxed(&lock->val, &val, (1U << 0)))
                                                                                                                                                                                                                                                                         goto release;
                                                                                                                                                                                                                                                                 }






                                                                                                                                                                                                                                                             set_locked(lock);




                                                                                                                                                                                                                                                             if (!next)
                                                                                                                                                                                                                                                                 next = ({ typeof(&node->next) __PTR = (&node->next); typeof(_Generic((*&node->next), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&node->next))) VAL; for (;;) { VAL = ({ do { __attribute__((__noreturn__)) extern void __compiletime_assert_450(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) __compiletime_assert_450(); } while (0); (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); }); if ((VAL)) break; cpu_relax(); } (typeof(*&node->next))VAL; });

                                                                                                                                                                                                                                                             do { do {} while (0); do { do { __attribute__((__noreturn__)) extern void __compiletime_assert_451(void) __attribute__((__error__("Need native word sized stores/loads for atomicity."))); if (!((sizeof(*(&next->locked)) == sizeof(char) || sizeof(*(&next->locked)) == sizeof(short) || sizeof(*(&next->locked)) == sizeof(int) || sizeof(*(&next->locked)) == sizeof(long)))) __compiletime_assert_451(); } while (0); __asm__ __volatile__("": : : "memory"); do { do { __attribute__((__noreturn__)) extern void __compiletime_assert_452(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); if (!((sizeof(*(&next->locked)) == sizeof(char) || sizeof(*(&next->locked)) == sizeof(short) || sizeof(*(&next->locked)) == sizeof(int) || sizeof(*(&next->locked)) == sizeof(long)) || sizeof(*(&next->locked)) == sizeof(long long))) __compiletime_assert_452(); } while (0); do { *(volatile typeof(*(&next->locked))*)& (*(&next->locked)) = (1); } while (0); } while (0); } while (0); } while (0);
                                                                                                                                                                                                                                                             __pv_kick_node(lock, next);

                                                                                                                                                                                                                                                         release:
                                                                                                                                                                                                                                                             trace_contention_end(lock, 0);




 ({ __this_cpu_preempt_check("add"); do { do { const void __seg_gs* __vpp_verify = (typeof((&(qnodes[0].mcs.count)) + 0))((void*)0); (void)__vpp_verify; } while (0); switch (sizeof(qnodes[0].mcs.count)) { case 1: do { const int pao_ID__ = (__builtin_constant_p(-(typeof(qnodes[0].mcs.count))(1)) && ((-(typeof(qnodes[0].mcs.count))(1)) == 1 || (-(typeof(qnodes[0].mcs.count))(1)) == (typeof(-(typeof(qnodes[0].mcs.count))(1))) - 1)) ? (int)(-(typeof(qnodes[0].mcs.count))(1)) : 0; if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pao_tmp__; pao_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pao_tmp__; } if (pao_ID__ == 1) ({ asm("inc" "b " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else if (pao_ID__ == -1) ({ asm("dec" "b " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else do { u8 pto_val__ = ((u8)(((unsigned long)-(typeof(qnodes[0].mcs.count))(1)) & 0xff)); if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pto_tmp__; pto_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pto_tmp__; } asm("add" "b " "%[val], " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count))) : [val] "qi" (pto_val__)); } while (0); } while (0); break; case 2: do { const int pao_ID__ = (__builtin_constant_p(-(typeof(qnodes[0].mcs.count))(1)) && ((-(typeof(qnodes[0].mcs.count))(1)) == 1 || (-(typeof(qnodes[0].mcs.count))(1)) == (typeof(-(typeof(qnodes[0].mcs.count))(1))) - 1)) ? (int)(-(typeof(qnodes[0].mcs.count))(1)) : 0; if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pao_tmp__; pao_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pao_tmp__; } if (pao_ID__ == 1) ({ asm("inc" "w " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else if (pao_ID__ == -1) ({ asm("dec" "w " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else do { u16 pto_val__ = ((u16)(((unsigned long)-(typeof(qnodes[0].mcs.count))(1)) & 0xffff)); if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pto_tmp__; pto_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pto_tmp__; } asm("add" "w " "%[val], " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count))) : [val] "ri" (pto_val__)); } while (0); } while (0); break; case 4: do { const int pao_ID__ = (__builtin_constant_p(-(typeof(qnodes[0].mcs.count))(1)) && ((-(typeof(qnodes[0].mcs.count))(1)) == 1 || (-(typeof(qnodes[0].mcs.count))(1)) == (typeof(-(typeof(qnodes[0].mcs.count))(1))) - 1)) ? (int)(-(typeof(qnodes[0].mcs.count))(1)) : 0; if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pao_tmp__; pao_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pao_tmp__; } if (pao_ID__ == 1) ({ asm("inc" "l " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else if (pao_ID__ == -1) ({ asm("dec" "l " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else do { u32 pto_val__ = ((u32)(((unsigned long)-(typeof(qnodes[0].mcs.count))(1)) & 0xffffffff)); if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pto_tmp__; pto_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pto_tmp__; } asm("add" "l " "%[val], " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count))) : [val] "ri" (pto_val__)); } while (0); } while (0); break; case 8: do { const int pao_ID__ = (__builtin_constant_p(-(typeof(qnodes[0].mcs.count))(1)) && ((-(typeof(qnodes[0].mcs.count))(1)) == 1 || (-(typeof(qnodes[0].mcs.count))(1)) == (typeof(-(typeof(qnodes[0].mcs.count))(1))) - 1)) ? (int)(-(typeof(qnodes[0].mcs.count))(1)) : 0; if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pao_tmp__; pao_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pao_tmp__; } if (pao_ID__ == 1) ({ asm("inc" "q " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else if (pao_ID__ == -1) ({ asm("dec" "q " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count)))); }); else do { u64 pto_val__ = ((u64)(-(typeof(qnodes[0].mcs.count))(1))); if (0) { __typeof_unqual__((qnodes[0].mcs.count)) pto_tmp__; pto_tmp__ = (-(typeof(qnodes[0].mcs.count))(1)); (void)pto_tmp__; } asm("add" "q " "%[val], " "%" "[var]" : [var] "+m" (((qnodes[0].mcs.count))) : [val] "re" (pto_val__)); } while (0); } while (0); break; default: __bad_size_call_parameter(); break; } } while (0); });
}
extern typeof(queued_spin_lock_slowpath) queued_spin_lock_slowpath; static void* __attribute__((__used__)) __attribute__((__section__(".discard.addressable"))) __UNIQUE_ID___addressable_queued_spin_lock_slowpath453 = (void*)(uintptr_t)&queued_spin_lock_slowpath; asm(".section \".export_symbol\",\"a\" ; __export_symbol_queued_spin_lock_slowpath: ; .asciz \"\" ; .ascii \"\" \"\\0\" ; .balign 8 ; .quad queued_spin_lock_slowpath ; .previous");

static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) __attribute__((__always_inline__)) 
u32 queued_fetch_set_pending_acquire(struct qspinlock* lock)
{
    u32 val;






    val = ({ 
        bool c; 
    asm __inline volatile (".pushsection .smp_locks,\"a\"\n" ".balign 4\n" ".long 671f - .\n" ".popsection\n" "671:" "\n\tlock " "btsl" " %[val], " "%[var]" "\n\t/* output condition code " "c" "*/\n" : [var] "+m" (lock->val.counter), "=@cc" "c" (c) : [val] "I" ((0 + 8)) : "memory"); 
    c; 
        })
        * (1U << (0 + 8));
    val |= atomic_read(&lock->val) & ~(((1U << 8) - 1) << (0 + 8));

    return val;
}

static inline __attribute__((__gnu_inline__)) __attribute__((__unused__)) __attribute__((no_instrument_function)) bool virt_spin_lock(struct qspinlock* lock)
{
    int val;

    if (!({ 
        bool branch; 
    if (__builtin_types_compatible_p(typeof(*&virt_spin_lock_key), struct static_key_true)) 
        branch = !arch_static_branch(&(&virt_spin_lock_key)->key, true); 
    else if (__builtin_types_compatible_p(typeof(*&virt_spin_lock_key), struct static_key_false)) 
        branch = !arch_static_branch_jump(&(&virt_spin_lock_key)->key, true); 
    else
        branch = ____wrong_branch_error(); __builtin_expect(!!(branch), 1); }))
        return false;







__retry:
    val = atomic_read(&lock->val);

    if (val || !atomic_try_cmpxchg(&lock->val, &val, (1U << 0))) {
        cpu_relax();
        goto __retry;
    }

    return true;
}
```

最重要的是这个宏

[smp_cond_load_acquire](https://elixir.bootlin.com/linux/v6.16.3/C/ident/smp_cond_load_acquire) 

smp_cond_load_acquire(&lock->locked, !VAL); 的展开如下

```
 ({ 
 typeof(_Generic((*&lock->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&lock->locked))) _val;
 _val = ({ typeof(&lock->locked) __PTR = (&lock->locked); 
           typeof(_Generic((*&lock->locked), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*&lock->locked))) VAL; 
           for (;;) { 
               VAL = ({ 
                   do { 
                       __attribute__((__noreturn__)) extern void __compiletime_assert_445(void) __attribute__((__error__("Unsupported access size for {READ,WRITE}_ONCE()."))); 
                       if (!((sizeof(*__PTR) == sizeof(char) || sizeof(*__PTR) == sizeof(short) || sizeof(*__PTR) == sizeof(int) || sizeof(*__PTR) == sizeof(long)) || sizeof(*__PTR) == sizeof(long long))) __compiletime_assert_445(); 
                   } while (0); 
               (*(const volatile typeof(_Generic((*__PTR), char: (char)0, unsigned char : (unsigned char)0, signed char : (signed char)0, unsigned short : (unsigned short)0, signed short : (signed short)0, unsigned int : (unsigned int)0, signed int : (signed int)0, unsigned long : (unsigned long)0, signed long : (signed long)0, unsigned long long : (unsigned long long)0, signed long long : (signed long long)0, default: (*__PTR)))*)& (*__PTR)); 
                   }); 
               if (!VAL) break;
               cpu_relax(); 
           } 
           (typeof(*&lock->locked))VAL; }); 
 do { 
     do {
     } while (0); 
     do { 
         do {
         } while (0); 
         __asm__ __volatile__("": : : "memory"); 
     } while (0); 
 } while (0); 
 (typeof(*&lock->locked))_val; });
```



# mutex

 [__rt_mutex_lock](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__rt_mutex_lock)  这个解释了信号量的一些东西 

死锁检测机制在哪呢？或者说被这个机制规避了？ 不用额外写一些代码了？
