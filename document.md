cd /cygdrive/d/project/gcc/gcc-15.1.0

./configure --build=x86_64-w64-mingw32   -disable-bootstrap STAGE1_C{,XX}FLAGS="-O0 -g"

cd /D/project/gcc/gcc-15.1.0/

./configure --host=x86_64-w64-mingw32 --build=x86_64-w64-mingw32 --target=x86_64-w64-mingw32  --enable-languages=c,c++,fortran,lto --with-gmp=/c/msys64/mingw64 --with-mpfr=/c/msys64/mingw64 --with-mpc=/c/msys64/mingw64 --with-isl=/c/msys64/mingw64 --with-native-system-header-dir=/mingw64/include  --enable-mingw-wildcard

/D/project/gcc/gcc-15.1.0/host-x86_64-w64-mingw32/gcc/xgcc -B/D/project/gcc/gcc-15.1.0/host-x86_64-w64-mingw32/gcc/ -L/mingw64/x86_64-w64-mingw32/lib -L/mingw64/mingw/lib -isystem /mingw64/x86_64-w64-mingw32/include -isystem /mingw64/mingw/include -B/mingw64/x86_64-w64-mingw32/bin/  -B/mingw64/x86_64-w64-mingw32/lib/../lib32/ -isystem /mingw64/x86_64-w64-mingw32/include -isystem /mingw64/x86_64-w64-mingw32/sys-include   -fno-checking -g -O2 -m32 -O2 -I../../.././libgcc/../winsup/w32api/include -g -O2 -DIN_GCC   -W -Wall -Wno-error=narrowing -Wwrite-strings -Wcast-qual -Wno-format -Wstrict-prototypes -Wmissing-prototypes -Wold-style-definition  -isystem ./include   -g -DIN_LIBGCC2 -fbuilding-libgcc -fno-stack-protector   -I. -I. -I../../../host-x86_64-w64-mingw32/gcc -I../../.././libgcc -I../../.././libgcc/. -I../../.././libgcc/../gcc -I../../.././libgcc/../include -I../../.././libgcc/config/libbid -DENABLE_DECIMAL_BID_FORMAT -DHAVE_CC_TLS -DUSE_EMUTLS -Wno-missing-prototypes -Wno-type-limits  -o addtf3.o -MT addtf3.o -MD -MP -MF addtf3.dep  -c ../../.././libgcc/soft-fp/addtf3.c

pacman -S mingw-w64-x86_64-isl
pacman -S mingw-w64-x86_64-gmp
pacman -S mingw-w64-x86_64-mpfr

./configure --host=x86_64-w64-mingw32 --build=x86_64-w64-mingw32 --target=x86_64-w64-mingw32  --enable-languages=c,c++,fortran,lto --with-gmp=/c/msys64/mingw64 --with-mpfr=/c/msys64/mingw64 --with-mpc=/c/msys64/mingw64 --with-isl=/c/msys64/mingw64 --with-native-system-header-dir=/mingw64/include  --enable-mingw-wildcard  --with-gnu-as --with-gnu-ld 

--prefix=/mingw64 --with-local-prefix=/mingw64/local --with-native-system-header-dir=/mingw64/include --libexecdir=/mingw64/lib --enable-bootstrap --enable-checking=release --with-arch=nocona --with-tune=generic --enable-mingw-wildcard --enable-languages=c,lto,c++,fortran,ada,objc,obj-c++,jit --enable-shared --enable-static --enable-libatomic --enable-threads=posix --enable-graphite --enable-fully-dynamic-string --enable-libstdcxx-backtrace=yes --enable-libstdcxx-filesystem-ts --enable-libstdcxx-time --disable-libstdcxx-pch --enable-lto --enable-libgomp --disable-libssp --disable-multilib --disable-rpath --disable-win32-registry --disable-nls --disable-werror --disable-symvers --with-libiconv --with-system-zlib --with-gmp=/mingw64 --with-mpfr=/mingw64 --with-mpc=/mingw64 --with-isl=/mingw64 --with-pkgversion='Rev8, Built by MSYS2 project' --with-bugurl=https://github.com/msys2/MINGW-packages/issues --with-gnu-as --with-gnu-ld --with-libstdcxx-zoneinfo=yes --disable-libstdcxx-debug --enable-plugin --with-boot-ldflags=-static-libstdc++ --with-stage1-ldflags=-static-libstdc++

./configure --with-native-system-header-dir=/ucrt64/include --disable-multilib --enable-threads=posix --with-boot-ldflags=-static-libstdc++ --with-stage1-ldflags=-static-libstdc++ --with-arch=nocona --with-tune=generic 
pacman -S mingw-w64-ucrt-x86_64-crt-git /cygdrive/d/project/gcc/cywgin/gcc-15.1.0

cd /cygdrive/d/project/gcc/cywgin/gcc-15.1.0

 /cygdrive/d/project/gcc/cywgin/gcc-15.1.0/configure --srcdir=/cygdrive/d/project/gcc/cywgin/gcc-15.1.0 --build=x86_64-pc-cygwin --host=x86_64-w64-mingw32 --target=x86_64-w64-mingw32   --prefix=/usr --exec-prefix=/usr --localstatedir=/var --sysconfdir=/etc  -C --without-libiconv-prefix --without-libintl-prefix --with-sysroot=/usr/x86_64-w64-mingw32/sys-root --with-build-sysroot=/usr/x86_64-w64-mingw32/sys-root --disable-multilib --disable-win32-registry --enable-languages=c,c++,fortran,lto,objc,obj-c++ --enable-fully-dynamic-string --enable-libgomp --enable-libquadmath --enable-libquadmath-support --enable-libssp --enable-version-specific-runtime-libs --enable-libgomp --enable-libada --with-dwarf2 --with-gcc-major-version-only --with-gnu-ld --with-gnu-as --with-tune=generic  --with-system-zlib --enable-threads=posix --libexecdir=/usr/lib

/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/configure --srcdir=/cygdrive/d/project/gcc/cywgin/gcc-15.1.0 --build=x86_64-pc-cygwin --host=x86_64-w64-mingw32 --target=x86_64-w64-mingw32   --prefix=/usr --exec-prefix=/usr --localstatedir=/var --sysconfdir=/etc  -C --without-libiconv-prefix --without-libintl-prefix --with-sysroot=/usr/x86_64-w64-mingw32/sys-root --with-build-sysroot=/usr/x86_64-w64-mingw32/sys-root --disable-multilib --disable-win32-registry --enable-languages=c,c++,fortran,lto,objc,obj-c++ --enable-fully-dynamic-string --with-gnu-ld --with-gnu-as --with-tune=generic  --with-system-zlib --enable-threads=posix --libexecdir=/usr/lib

x86_64-w64-mingw32-gcc -L/usr/x86_64-w64-mingw32/lib -L/usr/mingw/lib -isystem /usr/x86_64-w64-mingw32/include -isystem /usr/mingw/include --sysroot=/usr/x86_64-w64-mingw32/sys-root   -g -O2 -O2 -I/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc/../winsup/w32api/include -g -O2 -DIN_GCC   -W -Wall -Wno-error=narrowing -Wwrite-strings -Wcast-qual -Wstrict-prototypes -Wmissing-prototypes -Wold-style-definition  -isystem ./include   -g -DIN_LIBGCC2 -fbuilding-libgcc -fno-stack-protector   -I. -I. -I../.././gcc -I/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc -I/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc/. -I/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc/../gcc -I/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc/../include -I/cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc/config/libbid -DENABLE_DECIMAL_BID_FORMAT -DHAVE_CC_TLS -DUSE_EMUTLS  -o cpuinfo.o -MT cpuinfo.o -MD -MP -MF cpuinfo.dep  -c /cygdrive/d/project/gcc/cywgin/gcc-15.1.0/libgcc/config/i386/cpuinfo.c

 ./configure  --disable-bootstrap STAGE1_C{,XX}FLAGS="-O0 -g"

sudo mount -t ext4 -o loop  /mnt/c/Users/admin/linux/busybox-1.37.0/rootfs.img   /mnt/c/Users/admin/linux/busybox-1.37.0/fs
DESTDIR
qemu-system-x86_64 \
-kernel /mnt/c/Users/admin/linux/linux-5.15.78/arch/x86/boot/bzImage \
-hda /mnt/c/Users/admin/linux/busybox-1.37.0/rootfs.img \
-append "root=/dev/sda console=ttyS0" \
-nographic

HidP_GetScaledUsageValue

dequeue_entities

../configure --prefix=/opt/glibc-2.32

ld-linux-x86-64.so.2

libc.so.6
根子  /lib64/ld-linux-x86-64.so.2

cat > version-check.sh << "EOF"

sudo mount -v -t ext4 rootfs.img $LFS
chown -v lfs $LFS/{usr{,/*},var,etc,tools}

lfs下的lib和bin都是链接到usr底下的  应该是4.2.节出问题了

ln -sv pkgconf.1 /usr/share/man/man1/pkg-config.1  这好像不太对
8.20. Binutils-2.45  make -k check  这也报错了

Gawk-5.3.2

8.33Gettext安装报错
8.34. Bison-3.8.2 检查报错
m4 安装版本低了

这些必须得弄
mount -v -t ext4 rootfs.img $LFS
mknod -m 600 $LFS/dev/console c 5 1
mknod -m 666 $LFS/dev/null c 1 3
sudo mount -v --bind /dev $LFS/dev
sudo mount -vt devpts devpts $LFS/dev/pts -o gid=5,mode=620
sudo mount -vt proc proc $LFS/proc
sudo mount -vt sysfs sysfs $LFS/sys
sudo mount -vt tmpfs tmpfs $LFS/run

kpartx 的作用

8.78. Procps-ng-4.0.5检查报错

qemu-system-x86_64 test2.img
sudo grub-install --boot-directory=/mnt/boot/ /dev/loop0
kpartx  -av test.img

SET PATH=%PATH%;D:\software\Qt\Tools\mingw1310_64\bin;D:\software\Qt\Tools\CMake_64\bin

call D:\software\Qt\6.8.3\msvc2022_64\bin\qt-cmake.bat -G"Visual Studio 17 2022" . -DMySQL_INCLUDE_DIR="C:\Program Files\MySQL\MySQL Server 8.0\include" -DMySQL_LIBRARY="C:\Program Files\MySQL\MySQL Server 8.0\lib\libmysql.lib" -DCMAKE_INSTALL_PREFIX="D:\software\Qt\6.8.3\msvc2022_64"

# 构建一个简易版的linux Distrubution的步骤

```
dd if=/dev/zero of=rootfs.img bs=1M count=40000
fdisk rootfs.img
sudo kpartx -av rootfs.img
sudo mkfs.ext4 /dev/mapper/loop1p1
sudo mkfs.ext4 /dev/mapper/loop1p2
sudo mount /dev/mapper/loop1p1 /mnt/boot
sudo grub-install --root-directory=/mnt/boot/ /dev/loop1
qemu-system-x86_64 rootfs.img
vim /mnt/boot/boot/grub/grub.cfg
```

## 配置/boot/grub/grub.cfg

```
set default=0
set timeout=5

insmod part_gpt
insmod ext2
set root=(hd0,msdos1)

menuentry "GNU/Linux, Linux 6.16.1-lfs-12.4" {
 linux /boot/grub/bzImage root=/dev/sda2 ro
}
```

```
sudo cp linux-5.15.78/arch/x86/boot/bzImage /mnt/boot/boot/grub/
sudo mount /dev/mapper/loop1p2 $LFS
mkdir -pv $LFS/{etc,var} $LFS/usr/{bin,lib,sbin}

for i in bin lib sbin; do
 ln -sv usr/$i $LFS/$i
done

case $(uname -m) in
 x86_64) mkdir -pv $LFS/lib64 ;;
esac

mkdir -pv $LFS/{sources,tools,dev,proc,sys,run}

qemu-system-x86_64 -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage -hda /mnt/c/Users/admin/linux/rootfs.img -append "root=/dev/sda2 rw" -S -s

wget -r -np -nH https://mirrors.ustc.edu.cn/lfs/lfs-packages/12.4-rc1/ -e robots=off

按照lfs 第4章到第五章 编译安装好 就能用了
sudo mount -v --bind /dev $LFS/dev
sudo mount -vt devpts devpts -o gid=5,mode=0620 $LFS/dev/pts
sudo mount -vt proc proc $LFS/proc
sudo mount -vt sysfs sysfs $LFS/sys
sudo mount -vt tmpfs tmpfs $LFS/run
if [ -h $LFS/dev/shm ]; then
 sudo install -v -d -m 1777 $LFS$(realpath /dev/shm)
else
 sudo mount -vt tmpfs -o nosuid,nodev tmpfs $LFS/dev/shm
fi

sudo chroot "$LFS" /usr/bin/env -i \
 HOME=/root \
 TERM="$TERM" \
 PS1='(lfs chroot) \u:\w\$ ' \
 PATH=/usr/bin:/usr/sbin \
 MAKEFLAGS="-j$(nproc)" \
 TESTSUITEFLAGS="-j$(nproc)" \
 /bin/bash --login
```

# 几个常用的GDB语句

## 调试linux语句

```
file vmlinux
target remote:1234
```

## 断点

```
b kernel/sched/fair.c:1148 if $_streq($container_of(curr, "struct task_struct", "se")->comm,"sh")
```

## 显示源代码界面

```
layout src
```

## 只显示前几堆栈

```
  bt 7 //显示前7页
```

## 设置库搜索路径

当 GDB 无法显示共享库信息或显示信息有误时，通常是由于库搜索路径错误导致的。可以使用 *set sysroot*、*set solib-absolute-prefix* 和 *set solib-search-path* 来指定库搜索路径[1](https://blog.csdn.net/lixiangminghate/article/details/90754184)。

- *set sysroot* 和 *set solib-absolute-prefix* 是同一条命令，用于设置库的绝对路径前缀。

- *set solib-search-path* 用于设置动态库的搜索路径，可以设置多个路径，路径之间使用冒号（Linux）或分号（Windows）分隔。

例如：

(gdb) set sysroot /home/evan/gdbso/mips

(gdb) set solib-search-path /noexist:/home/evan/gdbso/mips

## 添加符号文件

符号文件也就是动态库

```
add-symbol-file  /usr/lib/libmount.so.1
```

# VIM-GDB调试代码

```
:Termdebug
```

## vim 窗口移动

```
ctrl+w+c  关闭窗口
CTRL+w K：将窗口移动到最上边（k，向上）。

CTRL+w H：将窗口移动到最左边（h，向左）。

CTRL+w J：将窗口移动到最下边（j，向下）。

CTRL+w L：将窗口移动到最右边（l，向右）。
```

## 能够让gdb窗口滚动起来

在Vim8以后，使用:ter 即可调出终端  
关于Vim8终端，跑过一段时间，想看前边的内容，怎么办？  
使用  
<Ctrl + w> <Shift + n>  
即可让终端进入Normal模式，然后就可以用Vim中Normal的键位去Navigate.  
然后可以使用  
<i>  
回到终端

## vim窗口改变大小

```
ctrl+w+加号  记住加号是shift+加号
```

# wsl中qemu安装debian

```
dd if=/dev/zero of=debian.img bs=1M count=20000
qemu-system-x86_64 -m 2048 -smp cores=2 -boot d -cdrom debian-13.1.0-amd64-netinst.iso -drive file=debian.img,if=virtio -net nic,model=virtio -net user -vga std
qemu-system-x86_64 -m 2048 -smp cores=2 -boot d -drive file=debian.img,if=virtio -net nic,model=virtio -net user -vga std
```

安装软件时记得选中国镜像第二个

记得安装gdb引导

# wsl中qemu安装debian（使用NVME硬盘）

```
dd if=/dev/zero of=nvme.img bs=1M count=20000

qemu-system-x86_64 -m 2048 -smp cores=2 -boot d -cdrom debian-13.1.0-amd64-netinst.iso -drive file=/mnt/c/Users/admin/linux/nvme.img,if=none,id=nvme0n1,format=raw -device nvme,drive=nvme0n1,serial=1234 -net nic,model=virtio -net user -vga std

qemu-system-x86_64 -m 2048 -smp cores=2 -boot d -drive file=/mnt/c/Users/admin/linux/nvme.img,if=none,id=nvme0n1,format=raw -device nvme,drive=nvme0n1,serial=1234
```



# qemu中使用自己的镜像启动debian

```
qemu-system-x86_64 -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage  -hda /mnt/c/Users/admin/linux/nvme.img -append "root=/dev/sda1 rw" -s -S
```

公司笔记本

```
qemu-system-x86_64 -kernel /mnt/c/Users/LANX/linux/linux-6.16.3/arch/x86/boot/bzImage  -hda /mnt/c/Users/LANX/linux/debian.img -append "root=/dev/sda1 rw" -s -S
```

自己电脑的

```
qemu-system-x86_64 -kernel /mnt/c/linux/linux-6.16.3/arch/x86/boot/bzImage  -hda /mnt/c/
linux/debian.img -append "root=/dev/sda1 rw" -s -S
```



# qemu中使用自己的镜像启动debian（nvme硬盘）





# qemu共享文件

宿主机输入这个

```
qemu-system-x86_64 -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage -hda /mnt/c/Users/admin/linux/debian.img -append "root=/dev/sda1 rw" -virtfs local,path=/mnt/c/Users/admin/linux/linux-6.16.3/mod,mount_tag=host0,security_model=passthrough,id=host0
```

虚拟机中输入这个

```
sudo mount -t 9p -o trans=virtio,version=9p2000.L host0 /mnt/share
```

注意  宿主机与虚拟机用户名得一致  要不然报操作不被允许  真离谱

# qemu单独启动debian

qemu-system-x86_64 -m 2048 -smp cores=2 -boot d -drive file=debian.img,if=virtio -net nic,model=virtio -net user -vga std



自己电脑的

qemu-system-x86_64  -m 4096 -smp cores=8  -hda /mnt/c/linux/debian.img

# proc文件系统调试函数

proc_task_lookup

proc_task_readdir

proc_pid_readdir

proc_register    //注册目录项用的

filesystems_proc_show  //ls /proc/filesystems 命令会调用到这

知乎的一篇文章不错([(21 封私信 / 30 条消息) Pid Namespace 原理与源码分析 - 知乎](https://zhuanlan.zhihu.com/p/335171876))

init_nsproxy (namespace的根节点)

查找文件 proc_get_inode

打开文件 proc_reg_open

[alloc_super](https://elixir.bootlin.com/linux/v6.16.3/C/ident/alloc_super) 用来创建superblock的

[inode_init_always_gfp](https://elixir.bootlin.com/linux/v6.16.3/C/ident/inode_init_always_gfp) 连接inode与[super_block](https://elixir.bootlin.com/linux/v6.16.3/C/ident/super_block) 的

proc_lookup_de  从这就能看出来dentry是VFS要查找目录先生成的 dir是他的父节点

# 自定义文件系统

## makefile内容

```
obj-m := main.o
all:
        make -C /mnt/c/Users/admin/linux/linux-6.16.3/modules/lib/modules/6.16.3/build M=$(PWD) modules
```

/mnt/c/Users/admin/linux/linux-6.16.3/modules/lib/modules/6.16.3/builds 是编译完sudo make modules_install INSTALL_MOD_PATH=/your/custom/path  安装的 也就是能在本机编译任何内核的module

WSL版本

```
#Makefile**********************************************************
obj-m := main.o
all:
    make -C /mnt/c/Users/admin/linux/WSL2-Linux-Kernel-linux-msft-wsl-6.6.y/modules/lib/modules/6.6.87.2-microsoft-standard-WSL2/build M=$(PWD) modules
```



## 相关命令

```
sudo insmod main.ko
sudo mount -t cstest fdfd mp

sudo umount mp
sudo rmmod main.ko
```



## 列出当前所有pid

```
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/kthread.h>
#include <linux/delay.h>
#include <linux/smp.h> 
#include <linux/fs.h>
#include <linux/nsproxy.h>
#include <linux/pid_namespace.h>
#include <linux/types.h>
#include <linux/errno.h>
#include <linux/tty.h>
#include <linux/tty_driver.h>
#include <linux/tty_flip.h>
#include <linux/serial.h>
#include <linux/timer.h>
#include <linux/string.h>
#include <linux/slab.h>
#include <linux/sched/signal.h>
#include <linux/wait.h>
#include <linux/bitops.h>
#include <linux/delay.h>
#include <linux/module.h>
#include <linux/serdev.h>
struct file_system_type cstest_fs_type = {
    .name = "cstest",
    .fs_flags        = FS_USERNS_MOUNT | FS_DISALLOW_NOTIFY_PERM    
};
struct tgid_iter {
    unsigned int tgid;
    struct task_struct *task;
};

static struct tgid_iter next_tgid(struct pid_namespace *ns, struct tgid_iter iter)
{
    struct pid *pid;

    if (iter.task)
        put_task_struct(iter.task);
    rcu_read_lock();
retry:
    iter.task = NULL;
    pid = find_ge_pid(iter.tgid, ns);
    if (pid) {
        iter.tgid = pid_nr_ns(pid, ns);
        iter.task = pid_task(pid, PIDTYPE_TGID);
        if (!iter.task) {
            iter.tgid += 1;
            goto retry;
        }
        get_task_struct(iter.task);
    }
    rcu_read_unlock();
    return iter;
}
static int __init init_hello_module(void)                //__init进行注明
{
    struct pid_namespace *ns=current->nsproxy->pid_ns_for_children;
    printk("pid_namespace address%d\n",ns);
    struct tgid_iter iter;
    iter.tgid = 0;
    iter.task = NULL;
    for (iter = next_tgid(ns, iter);
         iter.task;
         iter.tgid += 1, iter = next_tgid(ns, iter)){
        printk("cspid!%d \n",iter.tgid);    
    }    
    return 0;
}

static void __exit exit_hello_module(void)              //__exit进行注明
{

    return ;
}


MODULE_LICENSE("GPL");                             //模块许可证声明（必须要有）
module_init(init_hello_module);                    //module_init()宏，用于初始化
module_exit(exit_hello_module);                    //module_exit()宏，用于析构
```

## 注册文件系统

6.16.3那版

```
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/kthread.h>
#include <linux/delay.h>
#include <linux/smp.h> 
#include <linux/fs.h>
#include <linux/nsproxy.h>
#include <linux/pid_namespace.h>
#include <linux/types.h>
#include <linux/errno.h>
#include <linux/tty.h>
#include <linux/tty_driver.h>
#include <linux/tty_flip.h>
#include <linux/serial.h>
#include <linux/timer.h>
#include <linux/string.h>
#include <linux/slab.h>
#include <linux/sched/signal.h>
#include <linux/wait.h>
#include <linux/bitops.h>
#include <linux/delay.h>
#include <linux/module.h>
#include <linux/serdev.h>
#include <linux/fs_context.h>
#include <linux/user_namespace.h>
#include <linux/string.h>
static struct kmem_cache *proc_inode_cachep __ro_after_init;
static char* dataBuf;
static int datasize = 0;
static void init_once(void *foo)
{
	struct inode *ei = (struct inode *) foo;

	inode_init_once(ei);
}
static void proc_init_kmemcache(void)
{
	proc_inode_cachep = kmem_cache_create("proc_inode_cache",
					     sizeof(struct inode),
					     0, (SLAB_RECLAIM_ACCOUNT|
						SLAB_ACCOUNT|
						SLAB_PANIC),
					     init_once);
	dataBuf = kmalloc(100, GFP_KERNEL);
}
static struct inode *proc_alloc_inode(struct super_block *sb)
{
	struct inode *vfs_inode;
	vfs_inode = alloc_inode_sb(sb, proc_inode_cachep, GFP_KERNEL);
	if (!vfs_inode)
		return NULL;
	return vfs_inode;
}
static void proc_free_inode(struct inode *inode)
{
	kmem_cache_free(proc_inode_cachep,inode);
}
static void proc_evict_inode(struct inode *inode)
{
	truncate_inode_pages_final(&inode->i_data);
	clear_inode(inode);
}
static int proc_show_options(struct seq_file *seq, struct dentry *root)
{
	return 0;
}
const struct super_operations proc_sops = {
	.alloc_inode	= proc_alloc_inode,
	.free_inode	= proc_free_inode,
	.drop_inode	= generic_delete_inode,
	.evict_inode	= proc_evict_inode,
	.statfs		= simple_statfs,
	.show_options	= proc_show_options,
};
static void proc_kill_sb(struct super_block *sb)
{
	kill_anon_super(sb);
	return;
}
static void proc_fs_context_free(struct fs_context *fc)
{
	void *ctx = fc->fs_private;
	printk("ctx address  %p\n",ctx);
	return;
}
static int proc_parse_param(struct fs_context *fc, struct fs_parameter *param)
{
	return 0;
}


static int proc_reconfigure(struct fs_context *fc){
	struct super_block *sb = fc->root->d_sb;
	sync_filesystem(sb);
	return 0;
}
static int proc_root_getattr(struct mnt_idmap *idmap,
			     const struct path *path, struct kstat *stat,
			     u32 request_mask, unsigned int query_flags)
{
	generic_fillattr(&nop_mnt_idmap, request_mask, d_inode(path->dentry),
			 stat);
	return 0;
}
static int proc_pid_readdir(struct file *file, struct dir_context *ctx)
{
	if(ctx->pos==789){
		return 0;
	}
	char a='a';
	for(int i=1;i<5;i++){
		a++;
		dir_emit(ctx,&a,1,i,S_IFDIR);
	}
	ctx->pos=789;
	return 0;
}

static ssize_t cstestWrite (struct file* file, const char __user* ubuf, size_t size, loff_t* p) {
	datasize = size;
	char* buf = memdup_user_nul(ubuf, size);
	memcpy(dataBuf, buf, size);
	return size;
}
static ssize_t cstestRead(struct file* filp, char __user* buf, size_t siz, loff_t* ppos)
{
	int pos = *ppos;
	if (pos == datasize) {
		return 0;
	}
	int ret=copy_to_user(buf, dataBuf, datasize);
	if (ret) {
		return ret;
	}
	buf[datasize] = '\n';
	*ppos = datasize;
	return datasize;
}
static const struct file_operations proc_root_operations = {
	.read		= cstestRead,
	.write		= cstestWrite,
	.iterate_shared	 = proc_pid_readdir,
	.llseek		= generic_file_llseek,
};
static int inodenum=1;
static struct dentry *proc_root_lookup(struct inode * dir, struct dentry * dentry, unsigned int flags)
{
        
        struct inode *inode = new_inode(dir->i_sb);
        inode->i_private = 0;
        inode->i_ino = inodenum;
        inodenum++;
        simple_inode_init_ts(inode);
        char name=*(dentry->d_name.name);
        if((name-'a')%2==0){
            inode->i_mode = S_IRUGO|S_IFREG;
        }else{
            inode->i_mode = S_IWUGO|S_IFDIR;
        }
        inode->i_uid = GLOBAL_ROOT_UID;;
        inode->i_gid = GLOBAL_ROOT_GID;
        //inode->i_mtime.tv_sec=365;
        inode->i_mtime_sec=365;
	inode->i_fop = &proc_root_operations;
        return d_splice_alias(inode, dentry);
    
    return NULL;
}
/*
 * proc root can do almost nothing..
 */
static const struct inode_operations proc_root_inode_operations = {
	.lookup		= proc_root_lookup,
	.getattr	= proc_root_getattr,
};
static struct inode *proc_get_inode(struct super_block *sb)
{
	struct inode *inode = new_inode(sb);

	if (!inode) {
		return NULL;
	}

	inode->i_private = 0;
	inode->i_ino = inodenum;
    inodenum++;
	simple_inode_init_ts(inode);

	inode->i_mode = S_IFDIR | S_IRUGO | S_IXUGO;
	inode->i_uid = GLOBAL_ROOT_UID;;
	inode->i_gid = GLOBAL_ROOT_GID;
	

	if (S_ISREG(inode->i_mode)) {

	} else if (S_ISDIR(inode->i_mode)) {
		inode->i_op = &proc_root_inode_operations;
		inode->i_fop = &proc_root_operations;
	} else if (S_ISLNK(inode->i_mode)) {
		
	} else {
		BUG();
	}
	return inode;
}
static int proc_fill_super(struct super_block *s, struct fs_context *fc)
{

	/* User space would break if executables or devices appear on proc */
	s->s_iflags |= SB_I_USERNS_VISIBLE | SB_I_NOEXEC | SB_I_NODEV;
	s->s_flags |= SB_NODIRATIME | SB_NOSUID | SB_NOEXEC;
	s->s_blocksize = 1024;
	s->s_blocksize_bits = 10;
	s->s_magic = 1512421;
	s->s_op = &proc_sops;
	s->s_time_gran = 1;
	s->s_fs_info =0;
	s->s_stack_depth = FILESYSTEM_MAX_STACK_DEPTH;
	struct inode *root_inode;
	/* procfs dentries and inodes don't require IO to create */
	s->s_shrink->seeks = 0;
	root_inode=proc_get_inode(s);
	s->s_root = d_make_root(root_inode);
	return 0;
}
static int proc_get_tree(struct fs_context *fc)
{
	return get_tree_nodev(fc, proc_fill_super);
}
static const struct fs_context_operations proc_fs_context_ops = {
        .free           = proc_fs_context_free,
        .parse_param    = proc_parse_param,
        .get_tree       = proc_get_tree,
        .reconfigure    = proc_reconfigure,
}; 
static int proc_init_fs_context(struct fs_context *fc)
{
        struct pid_namespace *pid_ns = get_pid_ns(task_active_pid_ns(current));
        put_user_ns(fc->user_ns);
        fc->user_ns = get_user_ns(pid_ns->user_ns);
        put_pid_ns(pid_ns);
        fc->fs_private = (void*)1245122;
        fc->ops = &proc_fs_context_ops;
        return 0;
}
struct file_system_type cstest_fs_type = {
        .name = "cstest",
        .fs_flags               = FS_USERNS_MOUNT | FS_DISALLOW_NOTIFY_PERM,
        .init_fs_context        = proc_init_fs_context,
	.kill_sb		= proc_kill_sb,
	.fs_flags		= FS_USERNS_MOUNT | FS_DISALLOW_NOTIFY_PERM,
};
static int __init init_hello_module(void)                //__init进行注明
{
	proc_init_kmemcache();
        register_filesystem(&cstest_fs_type);
        return 0;
}
static void __exit exit_hello_module(void)              //__exit进行注明
{
        unregister_filesystem(&cstest_fs_type);
        return ;
} 
MODULE_LICENSE("GPL");                             //模块许可证声明（必须要有）
module_init(init_hello_module);                    //module_init()宏，用于初始化
module_exit(exit_hello_module);                    //module_exit()宏，用于析构

```

WSL版本  这个比上面新  明天验证一下

```
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/kthread.h>
#include <linux/delay.h>
#include <linux/smp.h> 
#include <linux/fs.h>
#include <linux/nsproxy.h>
#include <linux/pid_namespace.h>
#include <linux/types.h>
#include <linux/errno.h>
#include <linux/tty.h>
#include <linux/tty_driver.h>
#include <linux/tty_flip.h>
#include <linux/serial.h>
#include <linux/timer.h>
#include <linux/string.h>
#include <linux/slab.h>
#include <linux/sched/signal.h>
#include <linux/wait.h>
#include <linux/bitops.h>
#include <linux/delay.h>
#include <linux/module.h>
#include <linux/serdev.h>
#include <linux/fs_context.h>
#include <linux/user_namespace.h>
#include <linux/string.h>
static struct kmem_cache *proc_inode_cachep __ro_after_init;
static void init_once(void *foo)
{
	struct inode *ei = (struct inode *) foo;

	inode_init_once(ei);
}
static void proc_init_kmemcache(void)
{
	proc_inode_cachep = kmem_cache_create("proc_inode_cache",
					     sizeof(struct inode),
					     0, (SLAB_RECLAIM_ACCOUNT|
						SLAB_ACCOUNT|
						SLAB_PANIC),
					     init_once);
}
static struct inode *proc_alloc_inode(struct super_block *sb)
{
	struct inode *vfs_inode;
	vfs_inode = alloc_inode_sb(sb, proc_inode_cachep, GFP_KERNEL);
	if (!vfs_inode)
		return NULL;
	return vfs_inode;
}
static void proc_free_inode(struct inode *inode)
{
	kmem_cache_free(proc_inode_cachep,inode);
}
static void proc_evict_inode(struct inode *inode)
{
	truncate_inode_pages_final(&inode->i_data);
	clear_inode(inode);
}
static int proc_show_options(struct seq_file *seq, struct dentry *root)
{
	return 0;
}
const struct super_operations proc_sops = {
	.alloc_inode	= proc_alloc_inode,
	.free_inode	= proc_free_inode,
	.drop_inode	= generic_delete_inode,
	.evict_inode	= proc_evict_inode,
	.statfs		= simple_statfs,
	.show_options	= proc_show_options,
};
static void proc_kill_sb(struct super_block *sb)
{
	kill_anon_super(sb);
	return;
}
static void proc_fs_context_free(struct fs_context *fc)
{
	void *ctx = fc->fs_private;
	printk("ctx address  %p\n",ctx);
	return;
}
static int proc_parse_param(struct fs_context *fc, struct fs_parameter *param)
{
	return 0;
}


static int proc_reconfigure(struct fs_context *fc){
	struct super_block *sb = fc->root->d_sb;
	sync_filesystem(sb);
	return 0;
}
static int proc_root_getattr(struct mnt_idmap *idmap,
			     const struct path *path, struct kstat *stat,
			     u32 request_mask, unsigned int query_flags)
{
	generic_fillattr(&nop_mnt_idmap, request_mask, d_inode(path->dentry),
			 stat);
	return 0;
}
static int proc_pid_readdir(struct file *file, struct dir_context *ctx)
{
	if(ctx->pos==789){
		return 0;
	}
	char a='a';
	for(int i=1;i<5;i++){
		a++;
		dir_emit(ctx,&a,1,i,S_IFDIR);
	}
	ctx->pos=789;
	return 0;
}
static int proc_root_readdir(struct file *file, struct dir_context *ctx)
{
	return proc_pid_readdir(file, ctx);
}
static const struct file_operations proc_root_operations = {
	.read		 = generic_read_dir,
	.iterate_shared	 = proc_root_readdir,
	.llseek		= generic_file_llseek,
};
static int inodenum=1;
static struct dentry *proc_root_lookup(struct inode * dir, struct dentry * dentry, unsigned int flags)
{
        
        struct inode *inode = new_inode(dir->i_sb);
        inode->i_private = 0;
        inode->i_ino = inodenum;
        inodenum++;
        simple_inode_init_ts(inode);
        char name=*(dentry->d_name.name);
        if((name-'a')%2==0){
            inode->i_mode = S_IRUGO|S_IFREG;
        }else{
            inode->i_mode = S_IWUGO|S_IFDIR;
        }
        inode->i_uid = GLOBAL_ROOT_UID;;
        inode->i_gid = GLOBAL_ROOT_GID;
        inode->i_mtime.tv_sec=365;
        //inode->i_mtime_sec=365;
        return d_splice_alias(inode, dentry);
    
    return NULL;
}
/*
 * proc root can do almost nothing..
 */
static const struct inode_operations proc_root_inode_operations = {
	.lookup		= proc_root_lookup,
	.getattr	= proc_root_getattr,
};
static struct inode *proc_get_inode(struct super_block *sb)
{
	struct inode *inode = new_inode(sb);

	if (!inode) {
		return NULL;
	}

	inode->i_private = 0;
	inode->i_ino = inodenum;
    inodenum++;
	simple_inode_init_ts(inode);

	inode->i_mode = S_IFDIR | S_IRUGO | S_IXUGO;
	inode->i_uid = GLOBAL_ROOT_UID;;
	inode->i_gid = GLOBAL_ROOT_GID;
	

	if (S_ISREG(inode->i_mode)) {

	} else if (S_ISDIR(inode->i_mode)) {
		inode->i_op = &proc_root_inode_operations;
		inode->i_fop = &proc_root_operations;
	} else if (S_ISLNK(inode->i_mode)) {
		
	} else {
		BUG();
	}
	return inode;
}
static int proc_fill_super(struct super_block *s, struct fs_context *fc)
{

	/* User space would break if executables or devices appear on proc */
	s->s_iflags |= SB_I_USERNS_VISIBLE | SB_I_NOEXEC | SB_I_NODEV;
	s->s_flags |= SB_NODIRATIME | SB_NOSUID | SB_NOEXEC;
	s->s_blocksize = 1024;
	s->s_blocksize_bits = 10;
	s->s_magic = 1512421;
	s->s_op = &proc_sops;
	s->s_time_gran = 1;
	s->s_fs_info =0;
	s->s_stack_depth = FILESYSTEM_MAX_STACK_DEPTH;
	struct inode *root_inode;
	/* procfs dentries and inodes don't require IO to create */
	s->s_shrink.seeks = 0;
	root_inode=proc_get_inode(s);
	s->s_root = d_make_root(root_inode);
	return 0;
}
static int proc_get_tree(struct fs_context *fc)
{
	return get_tree_nodev(fc, proc_fill_super);
}
static const struct fs_context_operations proc_fs_context_ops = {
        .free           = proc_fs_context_free,
        .parse_param    = proc_parse_param,
        .get_tree       = proc_get_tree,
        .reconfigure    = proc_reconfigure,
}; 
static int proc_init_fs_context(struct fs_context *fc)
{
        struct pid_namespace *pid_ns = get_pid_ns(task_active_pid_ns(current));
        put_user_ns(fc->user_ns);
        fc->user_ns = get_user_ns(pid_ns->user_ns);
        put_pid_ns(pid_ns);
        fc->fs_private = (void*)1245122;
        fc->ops = &proc_fs_context_ops;
        return 0;
}
struct file_system_type cstest_fs_type = {
        .name = "cstest",
        .fs_flags               = FS_USERNS_MOUNT | FS_DISALLOW_NOTIFY_PERM,
        .init_fs_context        = proc_init_fs_context,
	.kill_sb		= proc_kill_sb,
	.fs_flags		= FS_USERNS_MOUNT | FS_DISALLOW_NOTIFY_PERM,
};
static int __init init_hello_module(void)                //__init进行注明
{
	proc_init_kmemcache();
        register_filesystem(&cstest_fs_type);
        return 0;
}
static void __exit exit_hello_module(void)              //__exit进行注明
{
        unregister_filesystem(&cstest_fs_type);
        return ;
} 
MODULE_LICENSE("GPL");                             //模块许可证声明（必须要有）
module_init(init_hello_module);                    //module_init()宏，用于初始化
module_exit(exit_hello_module);                    //module_exit()宏，用于析构

```



# WSL中安装自定义Kernel

## build kernel

1. Install the build dependencies:  
   `$ sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev cpio qemu-utils`

2. Modify WSL2 kernel configs (optional):  
   `$ make menuconfig KCONFIG_CONFIG=Microsoft/config-wsl`

3. Build the kernel using the WSL2 kernel configuration and put the modules in a `modules` folder under the current working directory:  
   `$ make KCONFIG_CONFIG=Microsoft/config-wsl && make INSTALL_MOD_PATH="$PWD/modules" modules_install`
   
   You may wish to include `-j$(nproc)` on the first `make` command to build in parallel.

## 修改.wslconfig文件（用户目录下）

[wsl2]
kernel=c:/Users/admin/linux/WSL2-Linux-Kernel-linux-msft-wsl-6.6.y/arch/x86/boot/bzImage

## 参考链接

[GitHub - microsoft/WSL2-Linux-Kernel: The source for the Linux kernel used in Windows Subsystem for Linux 2 (WSL2)](https://github.com/microsoft/WSL2-Linux-Kernel?tab=readme-ov-file)

[Advanced settings configuration in WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig)

# 在Linux中添加系统调用

找个确定能够被编译的内核代码文件 添加

```
SYSCALL_DEFINE1(xzy, int, a){
        return a+4;
}
```

在include/linux/syscalls.h中

```
asmlinkage long sys_xzy(int c);
```

在arch/x86/entry/syscalls/syscall_64.tbl中

```
454     common  xzy                     sys_xzy
```

include/uapi/asm-generic/unistd.h

```
#define __NR_xzy 454
__SYSCALL(__NR_xzy, sys_xzy)
```

kernel/sys_ni.c

```
COND_SYSCALL(xzy);
```

测试代码

```
#include <iostream>
#include <unistd.h>
#include <sys/syscall.h>
#include <errno.h>
int main() {
int fd = 78; // 标准输入文件描述符
char buffer[100];
ssize_t num_bytes;
std::cout<<__NR_xzy<<std::endl;
num_bytes = syscall(454, fd);
if (num_bytes > 0) {
std::cout << "读取到的内容：" << num_bytes << std::endl;
} else {
std::cout << "读取文件失败" <<errno<< std::endl;
}
return 0;
}
```

参考链接[Adding a New System Call — The Linux Kernel documentation](https://docs.kernel.org/process/adding-syscalls.html#syscall-generic-6-11)

# 关闭debian图形界面

1.打开grup配置文件

```
sudo nano /etc/default/grub
```

2.修改

GRUB_CMDLINE_LINUX=”” 修改为：GRUB_CMDLINE_LINUX=”text”

3.更新grub

```
sudo update-grub
```

4.更新系统服务管理器配置

```
sudo  systemctl set-default multi-user.target
```

5.重启

```
sudo init 6
```

# gdb调试内核module

参考链接[Debugging kernel and modules via gdb — The Linux Kernel documentation](https://docs.kernel.org/process/debugging/gdb-kernel-debugging.html)

能够在gdb调试内核

开启虚拟机与宿主机文件共享 使虚拟机能够加载模块

虚拟机加载模块

在宿主机编好ko文件后 放到源代码目录下

执行 

```
(gdb) lx-symbols
```

注意 得是虚拟机加载模块后 执行lx-symbols命令  这命令会自动在源码目录下查找加载的模块

```

```

# linux 内核编译

参考链接[How To Build Linux Kernel {Step-By-Step} | phoenixNAP KB](https://phoenixnap.com/kb/build-linux-kernel)

配置文件可以这样弄

1、执行硬件体系架构；由于最终是要使用QEMU来运行，因此直接将ARCH设置为x86，减少交叉编译所需要的时间。

```bash
export ARCH=x86
```

2、配置board config，同样是配置为x86架构的

```text
make  x86_64_defconfig
```

3、配置Linux内核

```text
make menuconfig
```

# linux的makefile

KBUILD_CFLAGS 这个好像管编译选项 我得查查     fuck！ 优化不了 因为代码中有涉及到只有优化才能编过去优化的代码 只能多看了

# 设置linux库查找路径

方法一：使用环境变量 LD_LIBRARY_PATH

这种方法不需要root权限，适用于临时设置。可以通过以下命令设置库文件路径：

```
export LD_LIBRARY_PATH=<your-lib-path>:$LD_LIBRARY_PATH
```

例如

```
export LD_LIBRARY_PATH=/opt/gtk/lib:$LD_LIBRARY_PATH
```

可以通过以下命令查看是否设置成功：

```
echo $LD_LIBRARY_PATH
```

这种方法的缺点是设置是临时的，重启或打开新的Shell后设置会失效。为了使设置永久生效，可以将上述命令添加到系统文件中，例如*/etc/profile*、*~/.bashrc*或*~/.bash_profile*。例如，将命令添加到*~/.bashrc*文件的末尾，然后使用以下命令使配置生效：

```
source ~/.bashrc
```

方法二：修改 /etc/ld.so.conf 文件

这种方法适用于永久设置库文件路径。可以将库文件的绝对路径添加到*/etc/ld.so.conf*文件中，每行一个路径。例如：

/usr/X11R6/lib

/usr/local/lib

/opt/lib

添加路径后，需要运行以下命令更新库缓存：

```
sudo /sbin/ldconfig
```

# mount 命令源码

前提：libmount.so.1 也是我重新编的 所以有符号信息能够进行调试

现在都装默认目录下了  怕个球头 大不了重装系统

加载模块文件挂上咱们的文件系统

```
sudo insmod /mnt/c/Users/admin/linux/mod/registerFileSystem/main.ko
```

进入代码目录

```
cd /usr/bin
```

相关gdb命令

```
file mount
add-symbol-file /lib/libmount.so.1
add-symbol-file /usr/local/lib/libc.so.6
set args -t cstest fdfd mp
b do_mount
b /mnt/c/Users/admin/linux/util-linux-2.41.1/libmount/src/context_mount.c:549
b hook_create_mount
b fsconfig
```

跟踪记录

```
(gdb) p call_hook
$2 = {int (struct libmnt_context *, struct hookset_hook *)} 0x7ffff7f76740 <call_hook>
hook_create_mount
```

# QT安装镜像命令

```
qt-online-installer-windows-x64-4.10.0.exe --mirror http://mirrors.ustc.edu.cn/qtproject
```

# 编译GLIBC

[下载链接](https://mirror-hk.koddos.net/lfs/lfs-packages/12.4/)

下载glibc-2.42-fhs-1.patch和glibc-2.42.tar.xz

```
patch -Np1 -i ../glibc-2.42-fhs-1.patch
mkdir -v build
cd       build
../configure                             \
      --prefix=/usr                      \
      --build=$(../scripts/config.guess) \
      --disable-nscd                     \
      libc_cv_slibdir=/usr/lib           \
      --enable-kernel=5.4
make -j8
make install
```

# 当前所用vimrc文件

```
if has('mouse')
        set mouse-=a
endif
set tabstop=4
set shiftwidth=4
set softtabstop=4
set expandtab
packadd! termdebug
:syntax enable
```

# 部署cesium项目

下载源码

解压

```
npm init -y
npm start
```

# cscope使用

建立索引文件

```
进入源码目录
cscope -b -v
```

在vim中加入数据索引文件

```
:cs add ./cscope.out
```

查找结构体定义

```
:cs f g 结构体名称
```

# 重装系统后需要的命令

```
sudo apt-get install vim
sudo -s
cat >> /root/.vimrc <<EOF
if has('mouse')
        set mouse-=a
endif
packadd! termdebug
EOF

sudo rm /etc/apt/sources.list
cat >> /etc/apt/sources.list <<EOF
deb http://ftp.cn.debian.org/debian/ trixie main contrib non-free non-free-firmware
EOF

sudo apt-get install make
sudo apt-get install gcc
```

# ls命令

## build

是这个package里的的coreutils-9.7

build 命令没记录 但可以参考[Linux From Scratch](https://www.linuxfromscratch.org/lfs/downloads/stable/LFS-BOOK-12.4-NOCHUNKS.html#ch-materials-packages) 这个里面

## gdb

基本代码都在这函数里面 

filemodestring  应该判断文件权限和类型的

print_current_files  输出获取的文件状态  基本从这就能反推inode各个字段是干啥的了

print_dir（打开文件并输出）

statx  获取状态的系统调用

do_lstat 获取状态  对应的系统调用  [do_statx](https://elixir.bootlin.com/linux/v6.16.3/C/ident/do_statx) 

```
cd /usr/local/bin
file /usr/local/bin/ls
b print_dir
b print_current_files
set args -l /mnt/c/Users/admin/linux/linux-6.16.3/mod/registerFileSystem/mp
```



# VFS

d_delete 为啥为1是不解锁？

unlink中解锁了

inode是谁提供的？

super_block  参考proc_get_inode



lookup 这接口是获取dentry的信息的  这个函数很有用  因为什么操作都会涉及到查  这个就是查的接口

第一个参数是父目录的dentry  第二个是要查的子文件的dentry  返回值没啥用 大部分情况为空

```
struct inode_operations {
	struct dentry * (*lookup) (struct inode *,struct dentry *, unsigned int);
	}
```

获取这个目录下的的文件列表

```
struct file_operations {
	int (*iterate_shared) (struct file *, struct dir_context *);
}
```



# WRITE_ONCE 和READ_ONCE

意义在哪？ 只是作为静态检查？那为什么 只有最后一个检查了  哦哦可能因为都一样 所以检查最后一个即可

# 查看宏展开的源代码

在源码目录下

```
find -name "*.o.cmd"
```

找到你要查看的源代码文件名称例如 ./fs/proc/.inode.o.cmd

vim打开它

找到gcc编译的那个配置 例如

```
gcc -Wp,-MMD,fs/proc/.inode.o.d -nostdinc -I./arch/x86/include -I./arch/x86/include/generated -I./include -I./include -I./arch/x86/include/uapi -I./arch/x86/include/generated/uapi -I./include/uapi -I./include/generated/uapi -include ./include/linux/compiler-version.h -include ./include/linux/kconfig.h -include ./include/linux/compiler_types.h -D__KERNEL__ -Werror -std=gnu11 -fshort-wchar -funsigned-char -fno-common -fno-PIE -fno-strict-aliasing -mno-sse -mno-mmx -mno-sse2 -mno-3dnow -mno-avx -fcf-protection=branch -fno-jump-tables -m64 -falign-jumps=1 -falign-loops=1 -mno-80387 -mno-fp-ret-in-387 -mpreferred-stack-boundary=3 -mskip-rax-setup -march=x86-64 -mtune=generic -mno-red-zone -mcmodel=kernel -mstack-protector-guard-reg=gs -mstack-protector-guard-symbol=__ref_stack_chk_guard -Wno-sign-compare -fno-asynchronous-unwind-tables -mindirect-branch=thunk-extern -mindirect-branch-register -mindirect-branch-cs-prefix -mfunction-return=thunk-extern -fno-jump-tables -fpatchable-function-entry=16,16 -fno-delete-null-pointer-checks -O2 -fno-allow-store-data-races -fstack-protector-strong -fomit-frame-pointer -ftrivial-auto-var-init=zero -fno-stack-clash-protection -fmin-function-alignment=16 -fstrict-flex-arrays=3 -fno-strict-overflow -fno-stack-check -fconserve-stack -fno-builtin-wcslen -Wall -Wextra -Wundef -Werror=implicit-function-declaration -Werror=implicit-int -Werror=return-type -Werror=strict-prototypes -Wno-format-security -Wno-trigraphs -Wno-frame-address -Wno-address-of-packed-member -Wmissing-declarations -Wmissing-prototypes -Wframe-larger-than=2048 -Wno-main -Wno-dangling-pointer -Wvla-larger-than=1 -Wno-pointer-sign -Wcast-function-type -Wno-array-bounds -Wno-stringop-overflow -Wno-alloc-size-larger-than -Wimplicit-fallthrough=5 -Werror=date-time -Werror=incompatible-pointer-types -Werror=designated-init -Wenum-conversion -Wunused -Wno-unused-but-set-variable -Wno-unused-const-variable -Wno-packed-not-aligned -Wno-format-overflow -Wno-format-truncation -Wno-stringop-truncation -Wno-override-init -Wno-missing-field-initializers -Wno-type-limits -Wno-shift-negative-value -Wno-maybe-uninitialized -Wno-sign-compare -Wno-unused-parameter -g -gdwarf-5    -DKBUILD_MODFILE='"fs/proc/proc"' -DKBUILD_BASENAME='"inode"' -DKBUILD_MODNAME='"proc"' -D__KBUILD_MODNAME=kmod_proc -c -o fs/proc/inode.o fs/proc/inode.c
```

复制出来 在后面加上

```
-save-temps=obj 
```

注意！！！得回到源代码根目录底下

```
gcc -Wp,-MMD,fs/proc/.inode.o.d -nostdinc -I./arch/x86/include -I./arch/x86/include/generated -I./include -I./include -I./arch/x86/include/uapi -I./arch/x86/include/generated/uapi -I./include/uapi -I./include/generated/uapi -include ./include/linux/compiler-version.h -include ./include/linux/kconfig.h -include ./include/linux/compiler_types.h -D__KERNEL__ -Werror -std=gnu11 -fshort-wchar -funsigned-char -fno-common -fno-PIE -fno-strict-aliasing -mno-sse -mno-mmx -mno-sse2 -mno-3dnow -mno-avx -fcf-protection=branch -fno-jump-tables -m64 -falign-jumps=1 -falign-loops=1 -mno-80387 -mno-fp-ret-in-387 -mpreferred-stack-boundary=3 -mskip-rax-setup -march=x86-64 -mtune=generic -mno-red-zone -mcmodel=kernel -mstack-protector-guard-reg=gs -mstack-protector-guard-symbol=__ref_stack_chk_guard -Wno-sign-compare -fno-asynchronous-unwind-tables -mindirect-branch=thunk-extern -mindirect-branch-register -mindirect-branch-cs-prefix -mfunction-return=thunk-extern -fno-jump-tables -fpatchable-function-entry=16,16 -fno-delete-null-pointer-checks -O2 -fno-allow-store-data-races -fstack-protector-strong -fomit-frame-pointer -ftrivial-auto-var-init=zero -fno-stack-clash-protection -fmin-function-alignment=16 -fstrict-flex-arrays=3 -fno-strict-overflow -fno-stack-check -fconserve-stack -fno-builtin-wcslen -Wall -Wextra -Wundef -Werror=implicit-function-declaration -Werror=implicit-int -Werror=return-type -Werror=strict-prototypes -Wno-format-security -Wno-trigraphs -Wno-frame-address -Wno-address-of-packed-member -Wmissing-declarations -Wmissing-prototypes -Wframe-larger-than=2048 -Wno-main -Wno-dangling-pointer -Wvla-larger-than=1 -Wno-pointer-sign -Wcast-function-type -Wno-array-bounds -Wno-stringop-overflow -Wno-alloc-size-larger-than -Wimplicit-fallthrough=5 -Werror=date-time -Werror=incompatible-pointer-types -Werror=designated-init -Wenum-conversion -Wunused -Wno-unused-but-set-variable -Wno-unused-const-variable -Wno-packed-not-aligned -Wno-format-overflow -Wno-format-truncation -Wno-stringop-truncation -Wno-override-init -Wno-missing-field-initializers -Wno-type-limits -Wno-shift-negative-value -Wno-maybe-uninitialized -Wno-sign-compare -Wno-unused-parameter -g -gdwarf-5    -DKBUILD_MODFILE='"fs/proc/proc"' -DKBUILD_BASENAME='"inode"' -DKBUILD_MODNAME='"proc"' -D__KBUILD_MODNAME=kmod_proc -c -o fs/proc/inode.o fs/proc/inode.c -save-temps=obj
```

即可在该目录底下看到inode.i 文件 该文件就是宏展开后的

# Debugging kernel and modules via gdb

[Debugging kernel and modules via gdb — The Linux Kernel documentation](https://docs.kernel.org/process/debugging/gdb-kernel-debugging.html)

# 打包QML项目

```
windeployqt.exe --qmldir D:\project\groundController D:\project\groundController\build\Desktop_Qt_6_8_3_MSVC2022_64bit-Release\groundStation.exe
```

--qmldir 后面跟的是源代码目录 注意！！是源！！代！！码！！根目录 不是exe目录

# 关于CPU核数确定

secondary_startup_64

set_nr_cpu_ids

[start_kernel](https://elixir.bootlin.com/linux/v6.16.3/C/ident/start_kernel)

topology_init_possible_cpus

[topo_get_cpunr](https://elixir.bootlin.com/linux/v6.16.3/C/ident/topo_get_cpunr)

acpi_parse_madt_lapic_entries

[acpi_parse_entries_array](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_parse_entries_array)

topo_info

acpi_parse_entries_array

topo_get_cpunr

/mnt/c/Users/admin/linux/linux-6.16.3/lib/fw_table.c:175

[set_freepointer](https://elixir.bootlin.com/linux/v6.16.3/C/ident/set_freepointer)

[allocate_slab](https://elixir.bootlin.com/linux/v6.16.3/C/ident/allocate_slab)

[do_slab_free](https://elixir.bootlin.com/linux/v6.16.3/C/ident/do_slab_free)

[build_detached_freelist](https://elixir.bootlin.com/linux/v6.16.3/C/ident/build_detached_freelist)

[get_freepointer](https://elixir.bootlin.com/linux/v6.16.3/C/ident/get_freepointer)

[__do_kmalloc_node](https://elixir.bootlin.com/linux/v6.16.3/C/ident/__do_kmalloc_node)

[kzalloc_noprof](https://elixir.bootlin.com/linux/v6.16.3/C/ident/kzalloc_noprof)

[acpi_tb_resize_root_table_list](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_tb_resize_root_table_list)

[acpi_allocate_root_table](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_allocate_root_table)



# grub手动添加内核



```
# 创建自定义脚本（40_ 前缀确保顺序）
sudo nano /etc/grub.d/40_custom

# 示例内容
#!/bin/sh
exec tail -n +3 $0

menuentry 'Custom Linux Kernel 5.15.0' {
    set root='hd0,msdos1'
    linux /boot/vmlinuz-custom root=/dev/sda1 ro
    initrd /boot/initrd-custom.img
}

```

写完然后执行两个命令

```
update-grub
grub-mkconfig -o /boot/grub/grub.cfg
```



# 退出网络共享账号

控制面板\用户帐户\凭据管理器

windows凭据  删除windows凭据



# git使用技巧

一般模式

```
git add .
git commit -m"balabala"
git push
```

撤销add

```
git restore --staged .
```

撤销所有修改

```
git checkout .
```



# QTCreator设置用系统编辑器打开MD文件

```
工具(Tools) → 选项(Options) → 环境(Environment) → MIME 类型(MIME Types)
```



# 无意间发现的神网址

[Fabrice Bellard's Home Page](https://bellard.org/)

# ACPI

[acpi_tb_parse_root_table](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_tb_parse_root_table) 

[21. ACPI Data Tables and Table Definition Language — ACPI Specification 6.4 documentation](https://uefi.org/htmlspecs/ACPI_Spec_6_4_html/21_ACPI_Data_Tables_and_Table_Def_Language/ACPI_Data_Tables.html) 



 [pci_mcfg_parse](https://elixir.bootlin.com/linux/v6.16.3/C/ident/pci_mcfg_parse)	很重要的东西  这个就是我找的  如何从acpi中找到pci的配置空间



[actbl2.h - include/acpi/actbl2.h - Linux source code v6.16.3 - Bootlin Elixir Cross Referencer](https://elixir.bootlin.com/linux/v6.16.3/source/include/acpi/actbl2.h#L37)  比较重要的一个文件  好多符号的宏定义都在这 以此类推actblX.h的这几个表都挺有用

 [ACPI_SIG_FADT](https://elixir.bootlin.com/linux/v6.16.3/C/ident/ACPI_SIG_FADT) 



 [acpi_db_set_method_data](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_db_set_method_data)  能够一窥object  和method的函数



[acpi_db_disassemble_method](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_db_disassemble_method) 反汇编的



[acpi_ns_lookup](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_ns_lookup)  查看找node的 这个肯定很重要

确定了一点就是AML是由操作系统执行的！！！但代码来源是固件开发者！！！牛逼！这不是虚拟机那一套吗哈哈可以，这种理念头一次被这么用哈哈  没啥毛病



[acpi_ps_execute_method](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_ps_execute_method)  看这个函数中的 [acpi_ps_parse_aml](https://elixir.bootlin.com/linux/v6.16.3/C/ident/acpi_ps_parse_aml) 函数  为啥叫“parse" 虚拟机！



# QT能够表示的时间

292278994年-2亿年多，感觉也还行。因为是用的64位，还不是全部范围 ，但也差不多。妈的64位才能表示这么大。自有后来人优化吧哈哈。不知道后来人会不会忘记这个事儿哈哈。

测试代码  QT6.8.3

```
//qint64 limitMsec=~((qint64)1<<63)-1000;
    QDateTime limitTime(QDate(292278994,1,1),QTime(0,1),Qt::LocalTime);
    if(limitTime.isValid()){
        QString limitStr=limitTime.toString();
        qint64 limitMsec=QDateTime::fromMSecsSinceEpoch(0).msecsTo(limitTime);
        qDebug()<<limitStr;
    }
```



