## openwrt 基础

[OpenWRT基本知识整理 – 寻梦嵌入式 (liwangmeng.com)](http://www.liwangmeng.com/openwrt基本知识归纳/#_Toc426631162)

目录结构说明

```shell
.
├── BSDmakefile				
├── config					# 存放着整个系统的的配置文件.
├── Config.in
├── CONTRIBUTED.md
├── COPYING
├── feeds					# 扩展软件包索引目录.
├── feeds.conf.default
├── include					# 编译规则
├── LICENSES
├── Makefile
├── package					# 整个系统的基础源码
├── README.md
├── rules.mk
├── run.sh
├── scripts					# 组织编译整个OpenWrt的规则.
├── staging_dir				# 是编译目标的最终安装位置, 其中包括rootfs, package, toolchain.
├── target					# 目标系统指嵌入式设备, 针对不同的平台有不同的特性, 针对这些特性, “target/linux”目录下按照平台进行目录划分, 里面包括了针对标准内核的补丁, 特殊配置等.
├── tmp						# 编译文件夹, 一般情况为空.临时目录, 用来储存不依赖于目标平台的工具.
├── toolchain				# 交叉编译工具
└── tools					# 工具

```



feed.conf.default

| Method       | Function                                                     |
| :----------- | :----------------------------------------------------------- |
| src-bzr      | Data is downloaded from the source path/URL using `bzr`      |
| src-cpy      | Data is copied from the source path. The path can be specified as either relative to OpenWrt repository root or absolute. |
| src-darcs    | Data is downloaded from the source path/URL using `darcs`    |
| src-git      | Data is downloaded from the source path/URL using `git` as a shallow (depth of 1) clone |
| src-git-full | Data is downloaded from the source path/URL using `git` as a full clone |
| src-gitsvn   | Bidirectional operation between a Subversion repository and git |
| src-hg       | Data is downloaded from the source path/URL using `hg`       |
| src-link     | A symlink to the source path is created. The path must be absolute. |
| src-svn      | Data is downloaded from the source path/URL using `svn`      |

### 构建

```shell
./scripts/feeds update -a # 更新feed.package
./scripts/feeds install -a # 安装到构建内容中去
make menuconfig
make defconfig # 检查依赖
 
make with -j1 V=s  
```



### Procd 守护进程

```shell
#!/bin/sh /etc/rc.common
USE_PROCD=1
START=95
STOP=01

start_service() {
    procd_open_instance
    procd_set_param command /bin/sh "/etc/netclient/netclient.sh"
    procd_close_instance
}
```



### openwrt  自编译指南

1. 为指定的设备添加引导配置 boot
2. 为设备设置指定参数 [target/linux/ramips/image/mt7621.mk](https://github.com/coolsnowwolf/lede/commit/5e34f402969b505f889dc491c9c67aa7346279a8#diff-5ac2666a9f7c325aa6d42d94872a91ebe7d8af73e2f708dc0f8564b0f3f88cd8)   make  menuconfig 菜单选项
3. 添加指定的dts 文件 
4. 为指定的basefile 定义led以及network 配置  [target/linux/ramips/mt7621/base-files/etc/board.d/01_leds](https://github.com/coolsnowwolf/lede/commit/5e34f402969b505f889dc491c9c67aa7346279a8#diff-09dce6818e981cc4f52d9d23e0b7326f762e85fa3281b2acd024653865fd0156)  02_network
5. 升级参数 未知  [target/linux/ramips/mt7621/base-files/lib/upgrade/platform.sh](https://github.com/immortalwrt/immortalwrt/commit/6d4382711a65b5606cc9b815e6590b0e36ebf5b5#diff-55c50ba2e40a6bc0b01dc1a06b85cfe21661f786aedd4b83f6aac46cc65f23c6) 

### TIPS

修改luci主题背景，feeds/luci文件夹中bg1.jpg图片

