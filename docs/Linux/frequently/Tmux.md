## Tmux 使用指北

### Tmux是什么

Tmux 是一款终端复用器

### 为什么要使用tmux

- 多窗口的痛处

  使用终端开启多个窗口进行交互，管理复杂平凡的使用鼠标才能进行窗口的切换，对于窗口多开支持非常不友好

- 断连的的痛处

  当使用终端连接意外断开时，终端上的任务意外断连后还需重新连接，重新执行一遍命令，非常耗费精力

### Tmux 能为我们提供什么

-   多窗口 / 多面板，同一终端下多个窗口
-   窗口自由分割
-   终端断连后保持会话
-   快捷键自由绑定
-   插件支持

### 概念

在Tmux中，需要分清楚Server > Session > Window > Pane 这个大小和层级顺序是极其重要的，直接关系到工作效率：

-   Server：是整个tmux的后台服务。有时候更改配置不生效，就要使用tmux kill-server来重启tmux。
-   Session [ˈseʃ(ə)n]：是tmux的使用基础，一个会话可以运行多个window。
-   Window：相当于一个工作区，包含很多分屏。一个session可以运行多个window。
-   Pane [peɪn]：是在Window里面的小分屏。



###  安装

```shell
# Mac os
brew install tmux

# Ubuntu
apt-get install tmux

# Centos
yum install tmux
```



### 使用流程

> 使用tmux 最重要的是快捷键的使用，尽量设置的顺手就行
>
> tmux 最好的学习手册为 man 手册  [tmux(1) - Linux manual page (man7.org)](https://www.man7.org/linux/man-pages/man1/tmux.1.html)
>
> ` ~ ?`

执行`tmux`命令会默认开启一个新的Session （会话）并进入到里面，进入到Session之后，展示的是一个window 窗口，

底部为tmux  Session 的状态栏，展示的信息包含 当前session 的的序号，当前window的序号。

**Tmux 所有的快捷键都基于前缀键（prefix key）**  快捷键都是 “prefix key” + "key"的形式

**Tmux 默认的prefix key 为 ‘C-b’ (Ctrl-b)**    以下使用 `~` 代替

Tmux 默认是禁用鼠标点击操作的，同vim。

基本操作：

- 从window 中分割出一块pane： `~  "` 水平分割  `~ %` 垂直分割 

  此时一块window分割成两个pane ：此时同时可以执行两个前台任务

- 切换不同的pane：`~ q + pane显示的数字` 

- 最大化pane：`~ z` ,最小化，再按一遍

- 关闭一个 pane ：

  - `~ x` 关闭命令，需要确认
  - `C - d` 退出命令，当剩下最后一个pane时会关闭当前Windows并detach 

- 保留会话退出Session： `~ d` 此时为detach ，会话转入后台运行

此时再次输入`tmux` 会创建一个新的Session，并attach 到此Session，此时状态栏序号会变成1，标明此为新的Session，退出此Session ， 执行`tmux ls` 显示有两个Session，并表明每个Session 使用的Window数量

- 重新进入上次的会话：  `tmxu a` 此时为 attach ，重新进入 最后退出的会话，`tmxu at -tn` n 为Session 的序号，表示attach 序号为n的Session
- 创建新的Window： `~ c` 创建新的Window 并attach ，此时状态栏* 号变到新的window下
- 切换window： `~ n` next window  `~ p` previous window , 快速切换window `~ n` n为window的序号
- 切换session：` ~ )` next session  ` ~ (` previous session, 切换任意session `~ s`   ? 如何切换任意window

### 命令行模式

`~ :` 进入命令行模式，可以执行tmux 命令

###  快捷键的绑定

**Tmux 默认的快捷键文件为 `~/.tmux.conf` **

tmux 所有的操作都可以用tmux 命令行执行，每个快捷键实际执行的是tmux特殊的命令

查看键的绑定关系 ` ~  ?`

键可以是组合，可以为单个键

- bind 语法，绑定某个指令到键
- unbind 语法，取消该键的绑定关系

example:

​	设置prefix 键  set -g prefix  `prefix_key`

​	取消默认的prefix 绑定  `unbind C-b`

​	绑定 | 为垂直分割  `bind | split-window -h -c "#{pane_current_path}"`

​	绑定 _ 为水平分割   ` bind _ split-window -v -c "#{pane_current_path}"`

### 插件系统 

tmux支持tmux插件管理

tpm

### 优缺点








???+ tip "光标丢失"
	`echo -e "\033[?25h"` 显示光标
	
	`echo -e "\033[?25l"` 隐藏光标

