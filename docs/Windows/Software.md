### Subline

**软件版本 4113 依此替换下方 2 组字节！（懒人直接下载修改好的：[下载地址](https://pan.ruyo.cc/软件/开发/Sublime Text/4)）**

```shell
C3 C6 01 00 C3 替换为 C3 C6 01 01 C3
51 31 C0 88 05 替换为 51 b0 01 88 05
```



## Windows 

> iso下载小技巧
>
> browser dev tools  ->  more tool -> network conditions ->  user agent -> change to chrome  
>
> then on the [downloadpage](https://www.microsoft.com/en-gb/software-download/windows10ISO) you can see the select option , step one by one , start  download



## 字体



## 必备软件

| 名称           | 链接                                                 | 描述                                                  |
| -------------- | ---------------------------------------------------- | ----------------------------------------------------- |
| Clash          | https://github.com/Fndroid/clash_for_windows_pkg     |                                                       |
| Git \ VIM      |                                                      |                                                       |
| Bandzip        | https://cn.bandisoft.com/bandizip/old/6/             | 超好用压缩软件                                        |
| Winaeroweaker  | https://winaero.com/winaero-tweaker/                 | windows美化                                           |
| Typora         | https://typora.io/                                   | 主题 https://github.com/kinoute/typora-hivacruz-theme |
| Geek           | https://geekuninstaller.com/download                 | 垃圾清理                                              |
| TrafficMonitor | https://github.com/zhongyang219/TrafficMonitor       | 网速工具                                              |
| Startisback    | https://www.startisback.com/#download-tab            | 任务栏美化                                            |
|                |                                                      |                                                       |
|                |                                                      |                                                       |
| Rufus          | https://rufus.ie/                                    | u盘安装工具                                           |
| Xshell         |                                                      |                                                       |
| DiskGenius     |                                                      |                                                       |
| Ventoy         | Github                                               | 系统安装工具                                          |
| Miniconda      | 依赖 https://slproweb.com/products/Win32OpenSSL.html |                                                       |

> `Git-bash`  `Pycharm`   `Tim`   `Wechat`  `Docker`  `向日葵` `Xmind` `Windows Terminal`

## Edge 插件相关

## Office365 三件套

## U盘相关

图片转换  [转换 (onlineconvertfree.com)](https://onlineconvertfree.com/zh/complete/svg-ico/)

根目录新建 `Autorun.inf` 文件 写入如下文件，并将ico文件重命名为`disk.ico`

```txt
[autorun]
icon=disk.ico
```

隐藏图片和配置文件 `attrib` 命令

```powershell
R 只读文件属性。
A 存档文件属性。
S 系统文件属性。
H 隐藏文件属性。

attrib +s +h +r 
```

