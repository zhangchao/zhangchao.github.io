# CentOS下Linux内核 #

9/9/2014 5:02:29 PM 

- 安装依赖


    yum install gcc make ncurses ncurses-devel perl

    yum update
    
> gcc gcc-c++  : make 编译需要的
> 
> ncurses-devel : make menuconfig 需要的

 
- 下载内核源码

    cd /tmp

    wget http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.9.tar.xz

    xz -d linux-3.9.tar.xz

    tar xf linux-3.9.tar -C /usr/src/
    

- 安装前配置

    cd /usr/src/linux-3.9/

    make menuconfig


> 按"ESC",点"YES"退出即可

    make oldconfig


> 为要编译的内核源码使用当前系统内核配置文件


> make mrproper : 完全清除 Kernel 资料夹内编译过和修改过的文件，包括「.config」

> make clean : 只会刪除中间文件


- 编译内核
 
	make -j4 all

> 为了加快编译速度，可以添加选项-jn，其中n替换为线程数值，一般是CPU核数的两倍。
 
> make bzImage（压缩内核）和make modules（编译模块），但是这两条命令无关紧要，可以不管

- 安装内核模块

	make modules_install install
 
> make all 指的是執行以下三個動作
>  
> make vmlinux : 未經壓縮的核心
> 
> make modules : 僅核心模組
> 
> make bzImage : 經壓縮過的核心

- 安装内核模块

	ls /boot/

	vim /etc/grub.conf

> change default to 0  
   
- 验证

	reboot

	uname -r
