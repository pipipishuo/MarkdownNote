声明网址  [3. ACPI Concepts — ACPI Specification 6.4 documentation](https://uefi.org/htmlspecs/ACPI_Spec_6_4_html/03_ACPI_Concepts/ACPI_Concepts.html)

哦对  ACPI是个声明  PCI总线是支持这个声明的  所以PCI的配置空间地址是通过ACPI表获取的

话说OSPM是写在固件里面的吗？不是 还是操作系统  有定义作为依据

- Operating System-directed Power Management (OSPM)

  A model of power (and system) management in which the OS plays a central role and uses global information to optimize system behavior for the task at hand.

# 2. Definition of Terms

## Platform

A platform consists of multiple devices assembled and working together to deliver a specific computing function, but does not include any other software other than the firmware as part of the devices in the platform. Examples of platforms include a notebook, a desktop, a server, a network switch, a blade, etc. - all without and independent of any operating system, user applications, or user data.

## OSPM

Operating System-directed configuration and Power Management

## System Control Interrupt (SCI)

A system interrupt used by hardware to notify the OS of ACPI events. The SCI is an active, low, shareable, level interrupt.

# 3. ACPI Concepts



### 3.3.1. Device Power Management Model

这句话比较重要  这么分层  按控制分层  头次听说按控制分层

**Layered device power state control**

Once power state decisions are made for a device, they must be carried-out by device drivers. The model partitions the control functionality between the device, bus and platform layers.

这个傻逼长难句，他说的意思是 Power Policy Owners能够确保只有这种状态能够被选择 什么样的状态呢？设备能够从这里苏醒的状态  哦哦哦  不能苏醒那不就傻逼了吗  Power Policy Owners也能决定何时设备需要唤醒系统

Power Policy Owners, which decide when the device might be needed to wake the system, ensure that only device power states that the device can wake from are selected when the platform enters a Sleep or LPI state. 

### 3.3.2. Power Management Standards

果不其然 看到上面的话我就觉得I/O interconnect说的是关于bus(PCI)的 也对 总线就是用来连接设备的  一点毛病都没有

I/O interconnect-level power management specifications are written for a number of buses including:

- PCI
- PCI Express
- CardBus
- USB
- IEEE 1394

### 3.3.3. Device Power States

这两个定义需要格外注意

- Device driver–What the device driver must do to restore the device to fully on.
- Restore latency–How long it takes to restore the device to fully on.

### 3.4.2. Setting Device Power States

有电源策略  不是随便直接设置的

For power-down operations (transitions from Dx to some deeper Dy), OSPM first evaluates the appropriate control method for the target state (_PSx), then turns-off any unused power resources

### 3.4.3. Getting Device Power Status

知道了第一个操作  能够知道电源策略和设备所处状态

OSPM uses the Get Power Status operation to determine the current power configuration (states and features), as well as the status of any batteries supported by the device.

### 3.4.4. Waking the System

看这样子还有bridges 也分层  人家叫上下游

Bus bridges must forward this signal to upstream bridges using the appropriate signal for that bus.

## 3.7. Configuration and “Plug and Play”

这几句很关键 他描述了“即插即用”的大体流程   还有配置功能  

In addition to power management, ACPI interfaces provide controls and information that enable OSPM to configure the required resources of motherboard devices along with their dynamic insertion and removal. ACPI Definition Blocks, including the Differentiated System Description Table (DSDT) and Secondary System Description Tables (SSDTs), describe motherboard devices in a hierarchical format called the ACPI namespace. The OS enumerates motherboard devices simply by reading through the ACPI Namespace looking for devices with hardware IDs.

Each device enumerated by ACPI includes ACPI-defined objects in the ACPI Namespace that report the hardware resources that the device could occupy, an object that reports the resources that are currently used by the device, and objects for configuring those resources. The information is used by the Plug and Play OS (OSPM) to configure the devices.

### 3.7.2. NUMA Nodes

关于涉及到NUMA的  但不多

Systems employing a Non Uniform Memory Access (NUMA) architecture contain collections of hardware resources including processors, memory, and I/O buses, that comprise what is commonly known as a “NUMA node”. Processor accesses to memory or I/O resources within the local NUMA node is generally faster than processor accesses to memory or I/O resources outside of the local NUMA node. ACPI defines interfaces that allow the platform to convey NUMA node topology information to OSPM both statically at boot time and dynamically at run time as resources are added or removed from the system.



## 3.8. System Events

这个案例非常好 说明了事件的处理流程

For example, assume a machine has all of its Plug and Play, Thermal, and Power Management events connected to the same pin in the core logic. The event status and event enable registers would only have one bit each: the bit corresponding to the event pin.

When the system is docked, the core logic sets the status bit and signals the SCI. The OS, seeing the status bit set, runs the control method for that bit. The control method checks the hardware and determines the event was a docking event (for example). It then signals to the OS that a docking event has occurred, and can tell the OS specifically where in the device hierarchy the new devices will appear.



### 3.9.1. Battery Communications

看来跟多简单的设备 也会有处理单元  并不只是CPU能进行运算

Both the Smart Battery and Control Method Battery interfaces provide a mechanism for the OS to query information from the platform’s battery system.



# 4. ACPI Hardware Specification

## 4.2. Fixed Hardware Programming Model

ACPI defines register-based interfaces to fixed hardware.



## 4.3. Generic Hardware Programming Model

PowerResource 的定义

As an example of a generic hardware control feature, a platform might be designed such that the IDE HDD’s D3 state has value-added hardware to remove power from the drive. The IDE drive would then have a reference to the AML **PowerResource** object (which controls the value added power plane) in its namespace, and associated with that object would be control methods that OSPM invokes to control the D3 state of the drive:



## 4.5. Register Bit Notation

一些注记符号表示

Throughout this section there are logic diagrams that reference bits within registers. These diagrams use a notation that easily references the register name and bit position. The notation is as follows:

> *Registername.Bit*
>
> *Registername* contains the name of the register as it appears in this specification
>
> *Bit* contains a zero-based decimal value of the bit position

For example, the SLP_EN bit resides in the PM1x_CNT register bit 13 and would be represented in diagram notation as:

> SLP_EN
>
> PM1x_CNT.13



G0是工作状态

G1是睡眠状态  睡眠状态是不会执行指令的

## 4.6. The ACPI Hardware Model



The ACPI architecture specifies some dedicated hardware not found in the legacy hardware model: the power management timer (PM Timer). This is a free running timer that the ACPI OS uses to profile system activity. The frequency of this timer is explicitly defined in this specification and must be implemented as described.



定义的这八个地址空间很重要  值得注意

Generic hardware features are manipulated by ACPI control methods residing in the ACPI Namespace. These interfaces can be very flexible; however, their use is limited by the defined ACPI control methods (for more information, see [ACPI-Defined Devices and Device-Specific Objects](https://uefi.org/htmlspecs/ACPI_Spec_6_4_html/09_ACPI-Defined_Devices_and_Device-Specific_Objects/ACPIdefined_Devices_and_DeviceSpecificObjects.html#acpi-defined-devices-and-device-specific-objects)). Generic hardware usually controls power planes, buffer isolation, and device reset resources. Additionally, “child” interrupt status bits can be accessed via generic hardware interfaces; however, they have a “parent” interrupt status bit in the GP_STS register. ACPI defines eight address spaces that may be accessed by generic hardware implementations. These include:

- System I/O space
- System memory space
- PCI configuration space
- Embedded controller space
- System Management Bus (SMBus) space
- CMOS
- PCI BAR Target
- IPMI space
- Platform Communication Channel

感觉这个FADT很有用  好多信息多是从这里拿到的 

## 4.8. ACPI Register Model

**4.8.3.1.1.2.** **Control Method Power Button**

这个就比较有用了  提到了标识符

If the power button is implemented using generic hardware, then the OEM needs to define the power button as a device with an _HID object value of “PNP0C0C,” which then identifies this device as the power button to OSPM. 

原来如此  说的按钮真就是真正的按钮  对呀 这不就是给硬件工程师看的吗



**4.8.3.1.2.2.** **Control Method Sleep Button**

这也能侧面说明generic hardware programming model和fix的区别

The sleep button programming model can also use the generic hardware programming model. This allows the sleep button to reside in any of the generic hardware address spaces (for example, the embedded controller) instead of fixed space. 

这才叫说明  你要支持 就必须这么实现

Although these bits are optional, if supported they must be implemented as described here.

### 4.8.5. Generic Hardware Registers

比较好理解的解释

Programming bits can reside in any of the defined generic hardware address spaces (system I/O, system memory, PCI configuration, embedded controller, or SMBus), but the top-level event bits are contained in the general-purpose event registers. The general-purpose event registers are pointed to by the GPE0_BLK and GPE1_BLK register blocks, and the generic hardware registers can be in any of the defined ACPI address spaces. A device’s generic hardware programming model is described through an associated object in the ACPI Namespace, which specifies the bit’s function, location, address space, and address location.



##### 4.8.5.1.1. General-Purpose Event 0 Register Block

这个值得注意

AML code cannot access the general-purpose event registers.



**4.8.5.1.1.2.** **General-Purpose Event 0 Enable Register**

按字节访问而不是整个长度  有意思

OSPM accesses GPE registers through byte accesses (regardless of their length).



# 5. ACPI Software Programming Model

## 5.1. Overview of the System Description Table Architecture

这句话很精辟，一句话就把Definition Block，data object，ACPI namespace的关系说清楚了

就是说 Definition Block是以data object的形式按层级结构放在ACPI namespace中的

A Definition Block contains information about the platform’s hardware implementation details in the form of data objects arranged in a hierarchical (tree-structured) entity known as the “ACPI namespace”, which represents the platform’s hardware configuration. 

看到没Data object是被用AML编码的

Data objects are encoded in a format known as ACPI Machine Language or AML for short. Data objects encoded in AML are “evaluated” by an OSPM entity known as the AML interpreter.

## 5.5. Control Methods and the ACPI Source Language (ASL)

OEMs and platform firmware vendors write definition blocks using the ACPI Source Language (ASL) and use a translator to produce the byte stream encoding described in [Definition Block Encoding](https://uefi.org/htmlspecs/ACPI_Spec_6_4_html/05_ACPI_Software_Programming_Model/ACPI_Software_Programming_Model.html#definition-block-encoding) .

