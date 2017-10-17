---
title: easy life 
---

## for China 

How to visit google:) [hoststools](https://github.com/HostsTools)


## Cygwin 

1. Install vim git git-completion openssh
2. Install lynx  
3. cygcheck -c cygwin

* lynx -source rawgit.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg
* install apt-cyg /bin

* apt-cyg install zsh curl git
* sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

但当我们开启第二个 Cygwin 窗口或者重启 Cygwin 的时候，默认使用的依然是 bash。那怎么样才能默认就使用 zsh 呢？
1. 在 .bashrc 文件最后添加代码 exec /bin/zsh，让 zsh 来替代 bash，就跟刚才我们直接执行 zsh 的操作一样
2. 右键点击 Cygwin 启动图标查看属性，更改其目标为 D:\cygwin64\bin\mintty.exe /bin/zsh -l。当然其中的路径需要修改成 Cygwin 的安装路径。

``` shell
cygpath -au 'C:\Windows\System32\drivers\etc'
cygpath -aw '/cygdrive/c/Windows/System32/drivers/etc'
```

打开文件或文件夹，并选中的脚本，可以这个脚本命名成xpf，放到PATH上。
# xpf是explorer and select file的缩写

``` shell
#!/bin/bash 
cygwin=false;
case "`uname`" in  CYGWIN*) cygwin=true ;;
esac

if [ "$1" = "" ]; then XPATH=. # 缺省是当前目录
else XPATH=$1
	if $cygwin; then XPATH="$(cygpath -C ANSI -w "$XPATH")";
	fi fi  explorer '/select,' $XPATH
```
