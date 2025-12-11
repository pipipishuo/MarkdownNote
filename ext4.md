[sd_init_command](https://elixir.bootlin.com/linux/v6.16.3/C/ident/sd_init_command)   从这打断点就明了了

[iomap_dio_submit_bio](https://elixir.bootlin.com/linux/v6.16.3/C/ident/iomap_dio_submit_bio)  直觉告诉我 这个比较重要  好像不太是

[__iomap_dio_rw](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__iomap_dio_rw)

[ext4_file_read_iter](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ext4_file_read_iter) 读的



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
