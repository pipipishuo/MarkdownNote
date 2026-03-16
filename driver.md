sr_probe

call_driver_probe

[cmos_pnp_probe](https://elixir.bootlin.com/linux/v6.16.3/C/ident/cmos_pnp_probe)

非常好的示例  我倒要看看是哪里把他调用的cmos_pnp_probe

pnp_device_probe

终究是迂回了一下



 [__pci_register_driver](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__pci_register_driver)

[pcie_portdriver](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pcie_portdriver)

[__pci_register_driver](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__pci_register_driver) 

[pci_bus_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_bus_type) pci总线类型

[pci_driver](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_driver) 写好自己的probe以后  通过[bus_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bus_type) 的probe函数 里面转换为pci_driver 如下

```
struct pci_dev *pci_dev = to_pci_dev(dev);
	struct pci_driver *drv = to_pci_driver(dev->driver);
```

然后就可以直接调用了 我说呢  总得有办法调用吧  那匹配肯定也是这个步骤了呗





[bus_add_device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bus_add_device)  这个应该是往总线中添加设备

关键属性[klist_devices](https://elixir.bootlin.com/linux/v6.16.3/C/ident/klist_devices)



这个函数应该是能够指导我找到设备热插拔时系统如何识别设备并添加到设备树中的

[pciehp_ist](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pciehp_ist)  

主要是这个函数  启动了一个内核线程[pciehp_poll](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pciehp_poll)

```
static inline int pciehp_request_irq(struct controller *ctrl)
{
	int retval, irq = ctrl->pcie->irq;

	if (pciehp_poll_mode) {
		ctrl->poll_thread = kthread_run(&pciehp_poll, ctrl,
						"pciehp_poll-%s",
						slot_name(ctrl));
		return PTR_ERR_OR_ZERO(ctrl->poll_thread);
	}

	/* Installs the interrupt handler */
	retval = request_threaded_irq(irq, pciehp_isr, pciehp_ist,
				      IRQF_SHARED, "pciehp", ctrl);
	if (retval)
		ctrl_err(ctrl, "Cannot get irq %d for the hotplug controller\n",
			 irq);
	return retval;
}
```





这个才是  扫描总线 上面那个应该是找打了以后的事

[pci_rescan_bus](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_rescan_bus) 



resacan初始的时候没被调用

 [pci_scan_child_bus](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_scan_child_bus)  应该是这个

这个应该是核心

[pci_setup_device](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_setup_device)



从这看 

[pci_scan_slot](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_scan_slot)



调用栈

```
#0  pci_setup_device (dev=dev@entry=0xffff888003926000) at drivers/pci/probe.c:1975
#1  0xffffffff81826d22 in pci_scan_device (bus=0xffff888003ab6400, devfn=0) at drivers/pci/probe.c:2577
#2  pci_scan_single_device (bus=0xffff888003ab6400, devfn=0) at drivers/pci/probe.c:2739
#3  pci_scan_single_device (bus=0xffff888003ab6400, devfn=0) at drivers/pci/probe.c:2729
#4  0xffffffff81826df1 in pci_scan_slot (bus=bus@entry=0xffff888003ab6400, devfn=devfn@entry=0)
    at drivers/pci/probe.c:2826
#5  0xffffffff8182808f in pci_scan_child_bus_extend (bus=bus@entry=0xffff888003ab6400,
    available_buses=available_buses@entry=0) at drivers/pci/probe.c:3045
```

[pci_conf1_read](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_conf1_read) 是比较重要的

这个代码解释了bus->devices从哪添加的

```
void pci_device_add(struct pci_dev *dev, struct pci_bus *bus)
{
....
list_add_tail(&dev->bus_list, &bus->devices);
....
}
```

这个提醒了我

[pcibios_device_add](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pcibios_device_add) 



其实查找设备本质上就是遍历一堆数字

```
unsigned int pci_scan_child_bus_extend(struct pci_bus *bus,
					      unsigned int available_buses){
...
for (devfn = 0; devfn < 256; devfn += 8)
		pci_scan_slot(bus, devfn);
...
					      }

```

[pci_read_config_byte](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_read_config_byte)  访问pci的函数



```
static struct pci_dev *pci_scan_device(struct pci_bus *bus, int devfn)
{
	struct pci_dev *dev;
	u32 l;

	/*
	 * Create pwrctrl device (if required) for the PCI device to handle the
	 * power state. If the pwrctrl device is created, then skip scanning
	 * further as the pwrctrl core will rescan the bus after powering on
	 * the device.
	 */
	if (pci_pwrctrl_create_device(bus, devfn))
		return NULL;

	if (!pci_bus_read_dev_vendor_id(bus, devfn, &l, 60*1000))
		return NULL;

	dev = pci_alloc_dev(bus);
	if (!dev)
		return NULL;

	dev->devfn = devfn;
	dev->vendor = l & 0xffff;
	dev->device = (l >> 16) & 0xffff;

	if (pci_setup_device(dev)) {
		pci_bus_put(dev->bus);
		kfree(dev);
		return NULL;
	}

	return dev;
}
```

[pci_bus_read_dev_vendor_id](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_bus_read_dev_vendor_id) 获取了vendor_id 和device_id  这两个值是设备自带的

pci_scan_device 是先用[pci_bus_read_dev_vendor_id](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_bus_read_dev_vendor_id)  获取完 然后赋值

```
	if (!pci_bus_read_dev_vendor_id(bus, devfn, &l, 60*1000))
		return NULL;

	dev = pci_alloc_dev(bus);
	if (!dev)
		return NULL;

	dev->devfn = devfn;
	dev->vendor = l & 0xffff;	//两句赋值
	dev->device = (l >> 16) & 0xffff;
```



一些关于pci配置空间的宏  从这些宏  你就能看见linux关于pci是如何处理的了

```
#define PCI_VENDOR_ID		0x00	/* 16 bits */
#define PCI_DEVICE_ID		0x02	/* 16 bits */
#define PCI_HEADER_TYPE		0x0e	/* 8 bits */
```

书《PCI_Express体系结构导读_王齐》2.3.2的那个图非常好

一些访问pci的宏 无外乎就是 访问的字节数量不同

```
PCI_USER_READ_CONFIG(byte, u8)
PCI_USER_READ_CONFIG(word, u16)
PCI_USER_READ_CONFIG(dword, u32)
PCI_USER_WRITE_CONFIG(byte, u8)
PCI_USER_WRITE_CONFIG(word, u16)
PCI_USER_WRITE_CONFIG(dword, u32)

PCI_OP_READ(byte, u8, 1)
PCI_OP_READ(word, u16, 2)
PCI_OP_READ(dword, u32, 4)
PCI_OP_WRITE(byte, u8, 1)
PCI_OP_WRITE(word, u16, 2)
PCI_OP_WRITE(dword, u32, 4)
```



这个函数也解决了如何判断设备存不存在

```
bool pci_bus_generic_read_dev_vendor_id(struct pci_bus *bus, int devfn, u32 *l,
					int timeout)
{
	if (pci_bus_read_config_dword(bus, devfn, PCI_VENDOR_ID, l))
		return false;

	/* Some broken boards return 0 or ~0 (PCI_ERROR_RESPONSE) if a slot is empty: */
	if (PCI_POSSIBLE_ERROR(*l) || *l == 0x00000000 ||
	    *l == 0x0000ffff || *l == 0xffff0000)
		return false;

	if (pci_bus_rrs_vendor_id(*l))
		return pci_bus_wait_rrs(bus, devfn, l, timeout);

	return true;
}
```

这个if 就说明设备存不存的依据  PCI_ERROR_RESPONSE   Vendor ID为0xFFFF说明无效  不存在肯定也属于无效 

参考链接[PCIe设备扫描避坑指南：从8B/10B编码原理到UEFI递归遍历的完整链路解析-CSDN博客](https://blog.csdn.net/weixin_29159127/article/details/158793556)

```
	/* Some broken boards return 0 or ~0 (PCI_ERROR_RESPONSE) if a slot is empty: */
	if (PCI_POSSIBLE_ERROR(*l) || *l == 0x00000000 ||
	    *l == 0x0000ffff || *l == 0xffff0000)
		return false;
```





x86把pci设为它的局部总线标准  说白了就是你的设备要是想在x86的cpu下执行  你就得能连在pci总线上  由此得到个结论  设备必须挂在总线上(这不废话吗)



acpi与pci的接口 都得来自acpi看来

```
struct pci_bus *pci_acpi_scan_root(struct acpi_pci_root *root)
```



acpi如何与pci相连的？  靠这个变量

```
static struct acpi_scan_handler pci_root_handler
```

[acpi_scan_add_handler](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_scan_add_handler)  添加用的

acpi怎么识别pci？





bus的集合[bus_kset](https://elixir.bootlin.com/linux/v6.16.3/C/ident/bus_kset)

usb总线的注册 [usb_bus_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/usb_bus_type)

[register_acpi_bus_type](https://elixir.bootlin.com/linux/v6.16.3/C/ident/register_acpi_bus_type) 连了acpi



[acpi_device_notify](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_device_notify)  看样子这个应该是枚举acpi设备都具体是啊啥的函数了

[usb_acpi_find_companion](https://elixir.bootlin.com/linux/v6.16.3/C/ident/usb_acpi_find_companion) 这个能够引导我一窥acpi与usb是怎么连接的

[acpi_bus_scan](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_bus_scan) acpi枚举设备--各种设备  处理器总线等等



CPU如何通过内存映射访问PCIE？

三个阶段：

1 探测所需空间，通过读取配置空间 [pci_conf1_read](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_conf1_read) 

2 写入配置  配置bar地址(物理地址)[pci_std_update_resource](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_std_update_resource) 

3 内存映射  (虚拟地址到物理地址的映射)[pci_iomap_range](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_iomap_range)



比方说

1 cpu读取pcie配置空间，探测到需要1M的地址空间

2 cpu决定将0xcccc作为该设备的起始物理地址 就将0xcccc写入bar寄存器

3 cpu将虚拟地址0xffff 映射到0xcccc   



此时cpu执行“往0xffff的虚拟地址写入1”的指令，经过地址转换  转换为“往 0xcccc的物理地址写入1”然后通过地址总线  找到0xcccc物理地址是该pci设备拥有  就往该pci设备写入了1
