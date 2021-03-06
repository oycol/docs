## Tmux 使用指北

### Tmux是什么

Tmux 是一款终端复用器

### 为什么要使用tmux

- 多窗口之敌

  使用终端开启多个窗口进行交互，管理复杂平凡的使用鼠标才能进行窗口的切换，对于窗口多开支持非常不友好

- 10053

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
# 使用3.0 及以上版本，不建议使用2.0版本
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

example：

-   开启鼠标点击 ` set -g mouse on`
-   关闭鼠标点击 ` set -g mouse off`

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

​	绑定鼠标快捷键  ` bind m set -g mouse on`

执行 `tmux source-file ~/.tmux.conf` 使配置生效  



### 插件系统

tmux支持插件功能，插件建议存储目录为 `~/.tmux/plugins`

- 备份恢复插件，tmux-resurrect  ，tmux-server 意外被kill之后 将窗口，会话，面板顺序 ，将`https://github.com/tmux-plugins/tmux-resurrect.git` 克隆到plugins目录中

  安装后需在`~/.tmux.conf`中增加一行配置：`run-shell ~/.tmux/plugins/tmux-resurrect/resurrect.tmux`

  保存 `~ C-s`  恢复 `~ C-r`  

  tmux会话的详细信息会以文本文件的格式保存到`~/.tmux/resurrect`目录

- 自动备份恢复插件，tmux-continuum（依赖Tmux Resurrect）， 安装方式

  `run-shell ~/.tmux/plugins/tmux-continuum/continuum.tmux`  具体参数设置参看文档

- 插件管理器， tpm， 初始安装方式同上

  使用tpm引入插件 ： 

  ```shell
  # 引入本身
  set -g @plugin 'tmux-plugins/tpm'
  # set -g @plugin 'github_username/plugin_name' # 格式：github用户名/插件名
  # set -g @plugin 'git@github.com/user/plugin' # 格式：git@github插件地址
  ```

  在插件配置底部添加如下行，初始化插件。

  ```shell
  run '~/.tmux/plugins/tpm/tpm'
  ```

  **~ r  快捷键可以重新加载配置** 

  安装插件 `~ I` tpm会自动下载插件，卸载插件，直接删除引入插件的行，`~ Alt u `

  



### 配置文件自定义

tmux有很多配置以及玩法都可以在tmux.conf 文件中配置，或者加入source-file 命令使单个文件加载多个文件的配置



### 会话多用户共享

支持多个不同用户同一个会话浏览



### 没有十全十美的Tmux

由于tmux高效的快捷键的使用，导致可能会和emacs,vim,nvim 等编辑器的快捷键冲突，由于本人对vim等编辑器的使用不深，暂时还没有碰到这类问题，欢迎有这类问题的同学交流意见

提问交流！！！

### 参考文档

>   https://man7.org/linux/man-pages/man1/tmux.1.html
>
>   https://www.cnblogs.com/zuoruining/p/11074367.html
>
>   http://blog.codepiano.com/2018/06/11/tmux-tpm
>
>   http://louiszhai.github.io/2017/09/30/tmux/#%E5%AF%BC%E8%AF%BB








???+ tip "光标丢失"
	`echo -e "\033[?25h"` 显示光标
	
	`echo -e "\033[?25l"` 隐藏光标

