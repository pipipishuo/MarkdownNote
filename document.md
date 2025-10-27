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

# qemu中使用自己的镜像启动debian

qemu-system-x86_64 -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage -hda /mnt/c/Users/admin/linux/debian.img -append "root=/dev/sda1 rw" -s -S

# qemu共享文件

宿主机输入这个

```
qemu-system-x86_64 -kernel /mnt/c/Users/admin/linux/linux-6.16.3/arch/x86/boot/bzImage -hda /mnt/c/Users/admin/linux/debian.img -append "root=/dev/sda1 rw" -virtfs local,path=/mnt/c/Users/admin/linux/mod,mount_tag=host0,security_model=passthrough,id=host0
```

虚拟机中输入这个

```
sudo mount -t 9p -o trans=virtio,version=9p2000.L host0 /mnt/shared
```

# qemu单独启动debian

qemu-system-x86_64 -m 2048 -smp cores=2 -boot d -drive file=debian.img,if=virtio -net nic,model=virtio -net user -vga std

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

# 模块demo

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
static struct kmem_cache *proc_inode_cachep __ro_after_init;
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
    .alloc_inode    = proc_alloc_inode,
    .free_inode    = proc_free_inode,
    .drop_inode    = generic_delete_inode,
    .evict_inode    = proc_evict_inode,
    .statfs        = simple_statfs,
    .show_options    = proc_show_options,
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
static struct dentry *proc_root_lookup(struct inode * dir, struct dentry * dentry, unsigned int flags)
{
        return NULL;
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
    char a='a';
    for(int i=1;i<5;i++){
        a++;
        dir_emit(ctx,&a,1,i,S_IFDIR | S_IRUGO | S_IXUGO);
    }
    return 0;
}
static int proc_root_readdir(struct file *file, struct dir_context *ctx)
{
    return proc_pid_readdir(file, ctx);
}
static const struct file_operations proc_root_operations = {
    .read         = generic_read_dir,
    .iterate_shared     = proc_root_readdir,
    .llseek        = generic_file_llseek,
};

/*
 * proc root can do almost nothing..
 */
static const struct inode_operations proc_root_inode_operations = {
    .lookup        = proc_root_lookup,
    .getattr    = proc_root_getattr,
};

static struct inode *proc_get_inode(struct super_block *sb)
{
    struct inode *inode = new_inode(sb);

    if (!inode) {
        return NULL;
    }

    inode->i_private = 0;
    inode->i_ino = 1;
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
    .kill_sb        = proc_kill_sb,
    .fs_flags        = FS_USERNS_MOUNT | FS_DISALLOW_NOTIFY_PERM,
};
static int __init init_hello_module(void)                //__init进行注明
{
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

mnt_context_do_mount

# QT安装

```
qt-online-installer-windows-x64-4.10.0.exe --mirror http://mirrors.ustc.edu.cn/qtproject
```
