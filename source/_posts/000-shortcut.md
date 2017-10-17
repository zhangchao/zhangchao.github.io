---
title: 000_shortcut
date: 2017-08-17 13:23:23
tags: shortcut
---

# Bash 
```shell
Alt + r：   撤销整行
Ctrl + u： 删除当前光标位置到行开始的字符
Ctrl + k： 删除当前光标位置到行末尾的字符

Ctrl + b： 向左移动一个字符
Ctrl + f： 向右移动一个字符
Alt + b： 向左移动一个单词
Alt + f： 向右移动一个单词
Ctrl + a: 移动行的开始
Ctrl + e： 移动行的末尾

Ctrl + p： 当前命令上一次执行的命令
```

# tmux  

```shell
tmux new -s session_name
tmux attach -t session_name 
tmux switch -t session_name 
tmux kill-session -t myname
tmux detach

Kill all the tmux sessions:
tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' |xargs kill

Ctrl + x 
+ f  find window 
+ c  create window 
+ ,  rename window
+ &   kill window

Ctrl + x
+ x  kill panel


```


# vim 

```shell

^ 行首
$ 行尾
b 前一个词
w 后一个词
( ) 句子
ctrl + f：向下移动一页
ctrl + b：向上移动一页

nG：n代表数字，例如：n=10，那么为移动到次文件的第10行
gg：移动到此文件的第一行
G：移动到次文件的最后一行


nx：向右删除n个字符
nX：向左删除n个字符
dd：删除当前行

:! cat 1.txt 显示 1.txt 文件的内容
```
 


# sublime 

```
Ctrl-D 选中同名同类 token
Ctrl + Alt + Down + Shift + Right :直到选中整个选区

Ctrl-N  new File 
Ctrl + W    关闭当前标签

Ctrl-Shift-p  打开Command Palette
Command + Shift + P
1. rename
2. Syntax HTML
3. Snippet @

Tab-->Space： 右下角  Convert Indentation to Tabs

Ctrl + D 选中一个单词
Ctrl + L 选中一行

Alt+Shift+1       Single             独屏
Alt+Shift+2       Columns:2      纵向二栏分屏

Ctrl + x
+ :  -->  setw synchronize-panes on  //同步所有panes

Ctrl + P    跳转到指定文件，输入文件名后可以：
1. @ 符号跳转    输入@symbol跳转到symbol符号所在的位置
2. # 关键字跳转    输入#keyword跳转到keyword所在的位置
3. : 行号跳转    输入:12跳转到文件的第12行。

```


# intellij
```
Alt + Insert 代码自动生成，比如setter、getter、toString等等
Shift + Shift 搜索所有文件
Ctrl + E 打开最近访问文件列表

Live Template :
fori


```

