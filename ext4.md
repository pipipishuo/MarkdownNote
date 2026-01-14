[sd_init_command](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sd_init_command)   从这打断点就明了了

[iomap_dio_submit_bio](https://elixir.bootlin.com/linux/v6.16.3/C/ident/iomap_dio_submit_bio)  直觉告诉我 这个比较重要  好像不太是

[__iomap_dio_rw](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__iomap_dio_rw)

[ext4_file_read_iter](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_file_read_iter) 读的

 [do_writepages](https://elixir.bootlin.com/linux/v6.16.3/C/ident/do_writepages)  目前看到这儿		2025.12.10 20:00



[filemap_get_read_batch](https://elixir.bootlin.com/linux/v6.16.3/C/ident/filemap_get_read_batch)  这个基本阐述了从内存中读取的理念  		2025.12.10 20:24

[ext4_find_extent](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_find_extent)  ext4的数据结构								2025.12.12 11：02

 [ext4_ext_try_to_merge](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_ext_try_to_merge)  									2025.12.12.19：33

[convert_initialized_extent](https://elixir.bootlin.com/linux/v6.16.3/C/ident/convert_initialized_extent)  这个也是在ext4_ext_map_blocks中的 2025.12.12.19：33

对应的mkdir调用栈

```
#0  ext4_find_extent (inode=inode@entry=0xffff888000855268, block=0, path=path@entry=0x0, flags=flags@entry=0)
    at fs/ext4/extents.c:889
#1  0xffffffff815f40d5 in ext4_ext_map_blocks (handle=handle@entry=0x0, inode=inode@entry=0xffff888000855268,
    map=map@entry=0xffffc9000043bd70, flags=flags@entry=0) at fs/ext4/extents.c:4208
#2  0xffffffff816087c6 in ext4_map_query_blocks (handle=handle@entry=0x0, inode=inode@entry=0xffff888000855268,
    map=map@entry=0xffffc9000043bd70, flags=flags@entry=0) at fs/ext4/inode.c:550
#3  0xffffffff8160b223 in ext4_map_blocks (handle=handle@entry=0x0, inode=inode@entry=0xffff888000855268,
    map=map@entry=0xffffc9000043bd70, flags=flags@entry=0) at fs/ext4/inode.c:775
#4  0xffffffff8162624c in ext4_append (handle=handle@entry=0xffff888003ce4188, inode=inode@entry=0xffff888000855268,
    block=block@entry=0xffffc9000043bdc4) at fs/ext4/namei.c:75
#5  0xffffffff8162c5ef in ext4_init_new_dir (handle=handle@entry=0xffff888003ce4188, dir=dir@entry=0xffff888003ee4590,
    inode=inode@entry=0xffff888000855268) at fs/ext4/namei.c:2969
#6  0xffffffff8162c797 in ext4_mkdir (idmap=0xffffffff82b7cfc0 <nop_mnt_idmap>, dir=0xffff888003ee4590,
    dentry=0xffff888003cf2a80, mode=<optimized out>) at fs/ext4/namei.c:3015
#7  0xffffffff81533ed0 in vfs_mkdir (idmap=0xffffffff82b7cfc0 <nop_mnt_idmap>, dir=0xffff888003ee4590,
    dentry=0xffff888003cf2a80, mode=<optimized out>) at fs/namei.c:4375
#8  0xffffffff815395de in do_mkdirat (dfd=dfd@entry=-100, name=0xffff888000bd3000, mode=mode@entry=511)
    at fs/namei.c:4408
```

AI告诉的

generic_file_read_iter

这是内核提供的通用函数，负责从页缓存中读取数据。如果数据不在缓存中，会触发缺页中断，将数据从磁盘加载到缓存。





[QEMU User Documentation — QEMU documentation](https://www.qemu.org/docs/master/system/qemu-manpage.html)  可以查查qemu的指令都是啥意思





```
qemu-system-x86_64   -drive file=/mnt/c/Users/admin/linux/debian.img,if=none,id=nvme0n1,format=raw  -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage  -append "root=/dev/sda1 rw" -device nvme,id=nvme_ctrl1,serial=NVME001 -device nvme-ns,drive=nvme0n1,bus=nvme_ctrl1 
```



```
qemu-system-x86_64 -machine q35 -drive file=/mnt/c/Users/admin/linux/debian.img,if=none,id=nvme0n1,format=raw  -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage  -append "root=/dev/sda1 rw" -device pcie-root-port,bus=pcie.0,id=port1,chassis=1 -device nvme,drive=nvme0n1,bus=port1
```



```
parted -l
```



```
qemu-system-x86_64 -m 2048 -smp cores=2 -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage  -drive file=/mnt/c/Users/admin/linux/nvme.img,if=none,id=nvme0n1,format=raw -device nvme,drive=nvme0n1,serial=1234 -append "root=/dev/nvme0n1p1" -s -S
```

写的相当清楚[固态硬盘选择别犯难：M.2、SATA、PCIe 和 NVMe先搞懂_硬盘_什么值得买](https://post.smzdm.com/p/am37p95d/)  

[ROOT_DEV](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ROOT_DEV) = [parse_root_device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/parse_root_device)([saved_root_name](https://elixir.bootlin.com/linux/v6.16.3/C/ident/saved_root_name));  目前看是这没找到咱想要的

 [prepare_namespace](https://elixir.bootlin.com/linux/v6.16.3/C/ident/prepare_namespace)

[blk_lookup_devt](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_lookup_devt)

[ilookup](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ilookup) 

[bdev_file_open_by_dev](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bdev_file_open_by_dev) 

[printk_all_partitions](https://elixir.bootlin.com/linux/v6.16.3/C/ident/printk_all_partitions) 

 [early_lookup_bdev](https://elixir.bootlin.com/linux/v6.16.3/C/ident/early_lookup_bdev)

root=UUID=c1cfe35e-d366-4f92-9107-2a10b60c7a0b ro text quiet

[__alloc_disk_node](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__alloc_disk_node) 这个应该就是绑定class的过程

[bdev_alloc](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bdev_alloc) 这个函数说明了 [bd_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bd_type) 文件系统是怎么和设备绑定在一起的



[bdev_cache_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bdev_cache_init) 设备文件系统初始化

[sr_probe](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sr_probe) 目前看到这了

bus_probe_device

# 几个类的关系

一个[subsys_private](https://elixir.bootlin.com/linux/v6.16.3/C/ident/subsys_private) 属于某一[class](https://elixir.bootlin.com/linux/v6.16.3/C/ident/class) 然后这个[subsys_private](https://elixir.bootlin.com/linux/v6.16.3/C/ident/subsys_private) 会有多个[device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device) 和[device_driver](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device_driver) 

但不是说这些[device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device)  都属于一个[device_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device_type) 

[subsys_private](https://elixir.bootlin.com/linux/v6.16.3/C/ident/subsys_private) 是由若干个不同[device_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device_type) 的[device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device) 组成的



其实[device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device) 也是个抽象类型，比如 [device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/device)  有[block_device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/block_device)  

[block_device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/block_device)  基本就是硬盘那一类的所以有个[gendisk](https://elixir.bootlin.com/linux/v6.16.3/C/ident/gendisk) 的成员

[gendisk](https://elixir.bootlin.com/linux/v6.16.3/C/ident/gendisk) 的[part0](https://elixir.bootlin.com/linux/v6.16.3/C/ident/part0) 成员连接到所属的[block_device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/block_device) 是一一对应的关系

证据代码

```
struct gendisk *__alloc_disk_node(struct request_queue *q, int node_id,
		struct lock_class_key *lkclass)
{
.......
disk->part0 = bdev_alloc(disk, 0);
.......
}
```

 [sr_probe](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sr_probe) 中最关键的代码应该是这句了

```
disk->fops = &sr_bdops;
```

[sd_fops](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sd_fops)  这个变量才有用 

 [sr_probe](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sr_probe) 应该是cd的 但我没不关注cd

 [scsi_ioctl](https://elixir.bootlin.com/linux/v6.16.3/C/ident/scsi_ioctl)  各种操作的函数

[blk_mq_dispatch_rq_list](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_dispatch_rq_list) 看样子像执行的函数

[scsi_commit_rqs](https://elixir.bootlin.com/linux/v6.16.3/C/ident/scsi_commit_rqs)  这个应该更像

[scsi_ioctl_sg_io](https://elixir.bootlin.com/linux/v6.16.3/C/ident/scsi_ioctl_sg_io) 

[blk_execute_rq](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_execute_rq) 

[sg_io](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sg_io) 

 [blk_mq_rq_to_pdu](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_rq_to_pdu) 这个很有意思啊 莫非request后面跟着个[scsi_cmnd](https://elixir.bootlin.com/linux/v6.16.3/C/ident/scsi_cmnd)

[blk_mq_rq_ctx_init](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_rq_ctx_init) 这里给mq_hctx成员赋值

[__blk_mq_alloc_requests](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__blk_mq_alloc_requests) 



[blk_mq_next_ctx](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_next_ctx) 

[INIT_DELAYED_WORK](https://elixir.bootlin.com/linux/v6.16.3/C/ident/INIT_DELAYED_WORK)(&[hctx](https://elixir.bootlin.com/linux/v6.16.3/C/ident/hctx)->[run_work](https://elixir.bootlin.com/linux/v6.16.3/C/ident/run_work), [blk_mq_run_work_fn](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_run_work_fn));

 [blk_mq_run_work_fn](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_run_work_fn) 

[__blk_mq_do_dispatch_sched](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__blk_mq_do_dispatch_sched) 

[__blk_mq_sched_dispatch_requests](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__blk_mq_sched_dispatch_requests)  这个是执行函数

[kblockd_workqueue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/kblockd_workqueue)  应该是个关键的变量

 [mod_delayed_work_on](https://elixir.bootlin.com/linux/v6.16.3/C/ident/mod_delayed_work_on) 

[blk_mq_delay_run_hw_queue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/blk_mq_delay_run_hw_queue) 



 [sd_getgeo](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sd_getgeo) 

[sd_ioctl](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sd_ioctl)

worker_thread



[ata_scsi_queuecmd](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ata_scsi_queuecmd)

[ata_qc_issue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ata_qc_issue) 

scsi_queue_rq 这是一个对外接口



 [ata_bmdma_qc_issue](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ata_bmdma_qc_issue)  艹TMD终于找到了 这才是

 [ata_sff_tf_load](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ata_sff_tf_load)  

[iowrite8](https://elixir.bootlin.com/linux/v6.16.3/C/ident/iowrite8) 最最最最最最底层函数

[ata_sff_data_xfer](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ata_sff_data_xfer) 





# 日志

[jbd2_journal_commit_transaction](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_commit_transaction) 总体函数



[jbd2_journal_write_revoke_records](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_write_revoke_records) 当前看的函数

[jbd2_journal_write_metadata_buffer](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_write_metadata_buffer) 

[jbd2_journal_commit_transaction](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_commit_transaction)

[__jbd2_journal_file_buffer](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__jbd2_journal_file_buffer) 

[do_get_write_access](https://elixir.bootlin.com/linux/v6.16.3/C/ident/do_get_write_access)

[jbd2_journal_get_write_access](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_get_write_access)

 [__ext4_journal_get_write_access](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__ext4_journal_get_write_access)

[ext4_journal_get_write_access](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_journal_get_write_access)

[ext4_ext_get_access](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_ext_get_access)

[convert_initialized_extent](https://elixir.bootlin.com/linux/v6.16.3/C/ident/convert_initialized_extent)

ext4_ext_map_blocks

底下就是mkdir的了

# jdb2

这段代码能够解决jbd2_journal_commit_transaction中的`while (commit_transaction->t_buffers)` 哪来的问题

```
void __jbd2_journal_file_buffer(struct journal_head *jh,
			transaction_t *transaction, int jlist){
			......
	case BJ_Metadata:
		transaction->t_nr_buffers++;
		list = &transaction->t_buffers;		
		.......
			}

```



0xffff888004378800

[jbd2_journal_start_thread](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_start_thread) 这个解释了 journal ext4 的关系

一个super对应一个日志系统  但日志系统有自己的线程  独立去做提交和处理事务  这就合理了 爽

[jbd2_journal_dirty_metadata](https://elixir.bootlin.com/linux/v6.16.3/C/ident/jbd2_journal_dirty_metadata) 算是ext4与jbd2的一个接口

# ext4

[__ext4_get_inode_loc](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__ext4_get_inode_loc)  这个函数非常好  解答了ext4文件系统的数据结构比如  哪个东西放哪里



#0  ext4_fill_raw_inode (inode=inode@entry=0xffff888003e7abe0, raw_inode=raw_inode@entry=0xffff888007f90900)
    at fs/ext4/inode.c:4744
#1  0xffffffff8160f211 in ext4_do_update_inode (iloc=0xffffc90000333bf0, handle=0xffff888003ce7000,
    inode=0xffff888003e7abe0) at fs/ext4/inode.c:5657
#2  ext4_mark_iloc_dirty (handle=handle@entry=0xffff888003ce7000, inode=inode@entry=0xffff888003e7abe0,
    iloc=iloc@entry=0xffffc90000333bf0) at fs/ext4/inode.c:6310
#3  0xffffffff8160fb6d in __ext4_mark_inode_dirty (handle=handle@entry=0xffff888003ce7000,
    inode=inode@entry=0xffff888003e7abe0, func=func@entry=0xffffffff82231be0 <__func__.2> "ext4_dirty_inode",
    line=line@entry=6545) at fs/ext4/inode.c:6516
#4  0xffffffff81613e46 in ext4_dirty_inode (inode=0xffff888003e7abe0, flags=<optimized out>) at fs/ext4/inode.c:6545



这个函数解答了怎么通过iloc找到ext4_rawinode的

```
static inline struct ext4_inode *ext4_raw_inode(struct ext4_iloc *iloc)
{
	return (struct ext4_inode *) (iloc->bh->b_data + iloc->offset);
}
```



[ext4_file_operations](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_file_operations) [ext4_dir_operations](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_dir_operations) 对vfs的文件和目录接口



# inode_table

[ext4_inode_table_set](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_inode_table_set) 

 [ext4_alloc_group_tables](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_alloc_group_tables) 这个可认为是设置inode_table的块号以及一些bitmap这个可以确定是在一个block group的开头

由[__ext4_get_inode_loc](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__ext4_get_inode_loc) 函数中的这块代码能确定inode_table一定是连续的  但并不能确定在创建文件时block是如何分配的  

```
block = ext4_inode_table(sb, gdp);
	if ((block <= le32_to_cpu(EXT4_SB(sb)->s_es->s_first_data_block)) ||
	    (block >= ext4_blocks_count(EXT4_SB(sb)->s_es))) {
		ext4_error(sb, "Invalid inode table block %llu in "
			   "block_group %u", block, iloc->block_group);
		return -EFSCORRUPTED;
	}
	block += (inode_offset / inodes_per_block);
```

[__ext4_new_inode](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__ext4_new_inode) 这里可以关注一下  看看是怎么创建文件的  知道怎么创建  就能观察block的分配了

[布局 — Linux 内核文档](https://docs.linuxkernel.org.cn/filesystems/ext4/blockgroup.html) 这个可作为ext4块组布局的参考  说白了 模式还是先确定好头部信息大小  然后去分配后面的数据块  也对  也只能这样 

# 块组

为什么一个块组大小默认是128M？

以为块位图占了一个块 一个块是4KB  也就是4096  也就是4096 * 8(32768)个bit  也就是这么多个块 也就是4096 * 8*4096字节 4096 * 8*4096就是128MB 

至于inode 就自己确定吧
