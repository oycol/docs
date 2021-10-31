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
make defconfig # 检查依赖
make menuconfig 
make with -j1 V=s  
```