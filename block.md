

[blk_mq_submit_bio](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_submit_bio) 

cond_resched



bio的一个[bi_end_io](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bi_end_io) 接口：mpage_end_io

  

kthread 这个函数估计会比较重要 是创造线程的



dd_bio_merge一个io调度器的



blk_execute_rq这个像是执行的



scsi_queue_rq这是个案例



blk_mq_alloc_queue  这应该是它的初始化  request队列



[nvme_start_request](https://elixir.bootlin.com/linux/v6.16.3/C/ident/nvme_start_request) 

[blk_mq_start_request](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_start_request) 这个是个关键

[scsi_queue_rq](https://elixir.bootlin.com/linux/v6.16.3/C/ident/scsi_queue_rq) 

# 细节片段

vfs发送bio到block层  这个基本是单向的

block经过一些步骤到驱动层  哪些步骤？ 通过[blk_mq_hw_ctx](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_hw_ctx) 里面会存硬件请求  然后下发 下发看这个[blk_mq_run_hw_queue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_run_hw_queue)  两种方式  一个是直接下发  一个异步下发

异步下发

```
#0  kblockd_mod_delayed_work_on (cpu=64, dwork=0xffff88800418da40, delay=0) at block/blk-core.c:1114
#1  0xffffffff8178a7ac in blk_mq_do_dispatch_sched (hctx=0xffff88800418da00) at block/blk-mq-sched.c:186
#2  __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00) at block/blk-mq-sched.c:307
#3  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00)
    at block/blk-mq-sched.c:329
#4  0xffffffff81780dd2 in blk_mq_run_hw_queue (hctx=hctx@entry=0xffff88800418da00, async=async@entry=false)
    at block/blk-mq.c:2356
```

直接下发

[blk_mq_sched_dispatch_requests](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_sched_dispatch_requests) 





很不错的博客[linux IO Block layer 解析 - 内核工匠 - 博客园](https://www.cnblogs.com/Linux-tech/p/12961286.html) 

其中这句话解开了我对何时下发请求给设备的疑惑

```
设备分发队列device dispatch q（也可以称作hardware dispatch q）

这是软件实现的队列。存储器件空闲时，其设备驱动程序主动从调度器中拉取一个rq存在设备分发队列中，分发队列中的rq按照先进先出顺序被封装成cmd下发给器件。

对于multi-queue，设备分发队列包中还额外包含per-core软件队列，它是为硬件分发队列服务的，可以把它理解成设备分发队列中的一部分。
```

好像不是  我没找到驱动程序怎么主动拉取rq



 [blk_mq_run_work_fn](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_run_work_fn) 但这个又是咋回事？ 莫非是双向的？既能设备自己去找  又能通过线程派发？

找到了 是这个接口 用来发布request的[dispatch_request](https://elixir.bootlin.com/linux/v6.16.3/C/ident/dispatch_request)  具体实现可以看这个[dd_dispatch_request](https://elixir.bootlin.com/linux/v6.16.3/C/ident/dd_dispatch_request) 

果然是io调度器管的  其实也对  插是往这里面插  拿肯定是从这里面拿



[kblockd_mod_delayed_work_on](https://elixir.bootlin.com/linux/v6.16.3/C/ident/kblockd_mod_delayed_work_on)  这是不是我找的通过线程去下发



这个调用堆栈就完美的说明了  程序是怎么走过来的  但这个是硬件队列 软件队列呢？

```
#0  scsi_queue_rq (hctx=0xffff88800418da00, bd=0xffffc90000063d58) at drivers/scsi/scsi_lib.c:1809
#1  0xffffffff81784255 in blk_mq_dispatch_rq_list (hctx=hctx@entry=0xffff88800418da00,
    list=list@entry=0xffffc90000063de0, get_budget=get_budget@entry=false) at block/blk-mq.c:2118
#2  0xffffffff8178a779 in __blk_mq_do_dispatch_sched (hctx=<optimized out>) at block/blk-mq-sched.c:168
#3  blk_mq_do_dispatch_sched (hctx=0xffff88800418da00) at block/blk-mq-sched.c:182
#4  __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00) at block/blk-mq-sched.c:307
#5  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00)
    at block/blk-mq-sched.c:329
#6  0xffffffff8177fd3c in blk_mq_run_work_fn (work=0xffff88800418da40) at block/blk-mq.c:2532
#7  0xffffffff8131837a in process_one_work (worker=worker@entry=0xffff888003967c00, work=0xffff88800418da40)
    at kernel/workqueue.c:3238
#8  0xffffffff8131903a in process_scheduled_works (worker=<optimized out>) at kernel/workqueue.c:3321
#9  worker_thread (__worker=0xffff888003967c00) at kernel/workqueue.c:3402
#10 0xffffffff81321e36 in kthread (_create=<optimized out>) at kernel/kthread.c:464
#11 0xffffffff812a9eb0 in ret_from_fork (prev=<optimized out>, regs=0xffffc90000063f58,
    fn=0xffffffff81321d40 <kthread>, fn_arg=0xffff8880039402c0) at arch/x86/kernel/process.c:148
#12 0xffffffff8126c2aa in ret_from_fork_asm () at arch/x86/entry/entry_64.S:245
```



从哪创建的呢？可参考这个

```
#0  kblockd_mod_delayed_work_on (cpu=64, dwork=0xffff88800418da40, delay=0) at block/blk-core.c:1114
#1  0xffffffff8178a7ac in blk_mq_do_dispatch_sched (hctx=0xffff88800418da00) at block/blk-mq-sched.c:186
#2  __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00) at block/blk-mq-sched.c:307
#3  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00)
    at block/blk-mq-sched.c:329
#4  0xffffffff81780dd2 in blk_mq_run_hw_queue (hctx=hctx@entry=0xffff88800418da00, async=async@entry=false)
    at block/blk-mq.c:2356
#5  0xffffffff81783acc in blk_mq_dispatch_list (rqs=rqs@entry=0xffffc900002479c0, from_sched=from_sched@entry=false)
    at block/blk-mq.c:2917
#6  0xffffffff81784864 in blk_mq_flush_plug_list (plug=<optimized out>, from_schedule=<optimized out>)
    at block/blk-mq.c:2965
#7  blk_mq_flush_plug_list (plug=plug@entry=0xffffc900002479c0, from_schedule=from_schedule@entry=false)
    at block/blk-mq.c:2937
#8  0xffffffff817750ce in __blk_flush_plug (plug=plug@entry=0xffffc900002479c0,
    from_schedule=from_schedule@entry=false) at block/blk-core.c:1220
#9  0xffffffff817754b7 in blk_finish_plug (plug=0xffffc900002479c0) at block/blk-core.c:1247
#10 __submit_bio (bio=bio@entry=0xffff888005363900) at block/blk-core.c:649
#11 0xffffffff81775881 in __submit_bio_noacct_mq (bio=0xffff888005363900) at block/blk-core.c:722
#12 submit_bio_noacct_nocheck (bio=<optimized out>) at block/blk-core.c:751
#13 0xffffffff816497f4 in __ext4_read_bh (bh=0xffff888003fae5b0, op_flags=12288, end_io=0x0, simu_fail=false)
    at fs/ext4/super.c:18
```



```
(gdb) frame 0
#0  kblockd_mod_delayed_work_on (cpu=64, dwork=0xffff88800418da40, delay=0) at block/blk-core.c:1114
1114    {
(gdb) p *dwork
$2 = {work = {data = {counter = 6291456}, entry = {next = 0xffff88800418da48, prev = 0xffff88800418da48},
    func = 0xffffffff8177fce0 <blk_mq_run_work_fn>}, timer = {entry = {next = 0xdead000000000122, pprev = 0x0},
    expires = 4294674838, function = 0xffffffff8131b5f0 <delayed_work_timer_fn>, flags = 98566144},
  wq = 0xffff888003a20800, cpu = 64}
```



通过plug 这个解决了我说的另一个问题  同一个线程一次只能发一个  怎么组合 ？ 其实是这样  多个线程往都同时块层发  因为对应的都是一个硬件  这时候就归块层管了  你发的我不一定现在就执行 我选择最优的执行

```
#0  scsi_queue_rq (hctx=0xffff88800418da00, bd=0xffffc90000263878) at drivers/scsi/scsi_lib.c:1809
#1  0xffffffff81784255 in blk_mq_dispatch_rq_list (hctx=hctx@entry=0xffff88800418da00,
    list=list@entry=0xffffc90000263900, get_budget=get_budget@entry=false) at block/blk-mq.c:2118
#2  0xffffffff8178a779 in __blk_mq_do_dispatch_sched (hctx=<optimized out>) at block/blk-mq-sched.c:168
#3  blk_mq_do_dispatch_sched (hctx=0xffff88800418da00) at block/blk-mq-sched.c:182
#4  __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00) at block/blk-mq-sched.c:307
#5  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00)
    at block/blk-mq-sched.c:329
#6  0xffffffff81780dd2 in blk_mq_run_hw_queue (hctx=hctx@entry=0xffff88800418da00, async=async@entry=false)
    at block/blk-mq.c:2356
#7  0xffffffff81783acc in blk_mq_dispatch_list (rqs=rqs@entry=0xffffc90000263aa0, from_sched=from_sched@entry=false)
    at block/blk-mq.c:2917
#8  0xffffffff81784864 in blk_mq_flush_plug_list (plug=<optimized out>, from_schedule=<optimized out>)
    at block/blk-mq.c:2965
#9  blk_mq_flush_plug_list (plug=plug@entry=0xffffc90000263aa0, from_schedule=from_schedule@entry=false)
    at block/blk-mq.c:2937
#10 0xffffffff817750ce in __blk_flush_plug (plug=0xffffc90000263aa0, from_schedule=from_schedule@entry=false)
    at block/blk-core.c:1220
#11 0xffffffff81775303 in blk_finish_plug (plug=plug@entry=0xffffc90000263aa0) at block/blk-core.c:1247
#12 0xffffffff81481fc5 in read_pages (rac=rac@entry=0xffffc90000263bc0) at mm/readahead.c:173
```



# 关键函数

blk_mq_run_work_fn

kblockd_mod_delayed_work_on

dd_dispatch_request

# 总结一下

多个线程通过plug 发送bio到block层   因为对应的都是一个硬件  这时候就归块层管了  你发的我不一定现在就执行 我选择最优的执行    然后下发 下发看这个[blk_mq_run_hw_queue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_run_hw_queue)  两种方式  一个是直接下发  一个异步下发

异步下发

```
#0  kblockd_mod_delayed_work_on (cpu=64, dwork=0xffff88800418da40, delay=0) at block/blk-core.c:1114
#1  0xffffffff8178a7ac in blk_mq_do_dispatch_sched (hctx=0xffff88800418da00) at block/blk-mq-sched.c:186
#2  __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00) at block/blk-mq-sched.c:307
#3  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff88800418da00)
    at block/blk-mq-sched.c:329
#4  0xffffffff81780dd2 in blk_mq_run_hw_queue (hctx=hctx@entry=0xffff88800418da00, async=async@entry=false)
    at block/blk-mq.c:2356
```

直接下发

[blk_mq_sched_dispatch_requests](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_sched_dispatch_requests) 



具体的一些片段  看上面的细节片段



最终谁执行





最新更新

是有两种模式下发  直接下发和异步下发  但都调用的一个函数blk_mq_dispatch_rq_list 这个函数有一句这个代码

```
bool blk_mq_dispatch_rq_list(struct blk_mq_hw_ctx *hctx, struct list_head *list,bool get_budget){
....
ret = q->mq_ops->queue_rq(hctx, &bd);
....
}
```



无论是直接下发还是异步下发都是从这blk_mq_run_hw_queue函数开始的

## 直接下发

调用栈

```
#0  blk_mq_dispatch_rq_list (hctx=hctx@entry=0xffff888004198a00, list=list@entry=0xffffc900000d3af8,
    get_budget=get_budget@entry=true) at block/blk-mq.c:2088
#1  0xffffffff8178a401 in __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff888004198a00)
    at block/blk-mq-sched.c:299
#2  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff888004198a00)
    at block/blk-mq-sched.c:329
#3  0xffffffff81780dd2 in blk_mq_run_hw_queue (hctx=hctx@entry=0xffff888004198a00, async=async@entry=false)
```



## 异步下发

调用栈

```
#0  __blk_mq_do_dispatch_sched (hctx=<optimized out>) at block/blk-mq-sched.c:92
#1  blk_mq_do_dispatch_sched (hctx=0xffff888004198a00) at block/blk-mq-sched.c:182
#2  __blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff888004198a00) at block/blk-mq-sched.c:307
#3  0xffffffff8178a9c8 in blk_mq_sched_dispatch_requests (hctx=hctx@entry=0xffff888004198a00)
    at block/blk-mq-sched.c:329
#4  0xffffffff8177fd3c in blk_mq_run_work_fn (work=0xffff888004198a40) at block/blk-mq.c:2532
```

[blk_mq_dispatch_rq_list](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_dispatch_rq_list) 这里面有调用[](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_dispatch_rq_list) 的代码

```
static int __blk_mq_do_dispatch_sched(struct blk_mq_hw_ctx *hctx)
{
    .....
    dispatched = blk_mq_dispatch_rq_list(hctx, &rq_list, false);
    .....
}
```

