## ArchLinux

### 安装

> archlinux只支持利用启动iso联网安装最新版本

1. 进入archlinux iso 引导安装界面

2. 设置按键映射

    ```shell
    ls /usr/share/kbd/keymaps/**/*.map.gz
    ```

    


3. 手动设置archlinux联网环境，配置文件 [`/etc/systemd/network`](https://wiki.archlinux.org/title/systemd-networkd)

4. 磁盘分区

    - 创建GPT分区表  / uefi 启动分区  /  主分区
    - 开启uefi
    
    ```shell
    mklable gpt
    mkpart "EFI system partition" fat32 1M 260M
    mkpart "Arch" ext4 260M 100%
    set 1 esp on
    ```

    !!! info "parted没有执行分区格式化  / 懒得看了" 
        分区设置用parted时 ,退出parted时需要重新格式化相应的分区
    
5. 挂载主分区到**mnt**目录，新建`/mnt/efi`文件夹，挂载efi分区到`/mnt/efi`

6. 安装系统内核
    ```shell
    pacstrap /mnt base linux linux-firmware neovim # 安装neovim 以便编辑文件
    ```
    
7. 将分区信息写入到新系统的fstab文件中
    ```shell
    genfstab -U /mnt >> /mnt/etc/fstab
    ```
    
8. 复制网络配置文件
    ```shell
    cp /etc/systemd/network/20* /mnt/etc/systemd/network/
    ```
    
9. 进入系统进行初始化设置

    ```shell
    arch-chroot /mnt
    ```

    ---

10. 时间   / 地区  / 语言

    ```shell
    timedatectl set-timezone Asia/Shanghai
    ln -sf /usr/share/zoneinfo/Asia/Shangahi /etc/localtime
    hwclock --systohc
    ```

11. 网络

     - `/etc/hostname`
     - `/etc/hosts`

12. 设置root密码

13. 安装系统引导  /  指定的cpu微码   /  生成启动配置文件

     === "UEFI引导"
          
          ```shell
          pacman -S  grub efibootmgr
          grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
          pacman -S intel-ucode/amd-ucode
          grub-mkconfig -o /boot/grub/grub.cfg
          ```

     === "BIOS引导" 
         

          ```shell
          pacman -S grub
          grub-install  /dev/sd # 指定的分区盘符
          grub-mkconfig -o /boot/grub/grub.cfg
          ```
         !!! info
              可以参照GPT分区格式，也可以直接使用MBR分区格式，简单，磁盘头部预留1-2M空间给MBR记录使用

14. 重启系统

    ```shell
    exit    # 退出chroot
    umount -R /mnt # 卸载分区
    reboot
    ```

### 使用

桌面环境

```shell
pacman -S plasma-desktop plasma-wayland-session sddm

pacman -S kde-applications
```



使用应用

```shell
[default]
sudo pacman -S --needed base-devel
# 字体
 - ttf-fira-code
 - 
# 声音模块
 - alsa-utils
# FileBowser 
	- dolphin
# FileSearching Engine
	
# Console
	- kitty
# Bowser
	- Edge
	- aur microsoft-edge-dev-bin

# Archive managers
	- 7z
# Proxy 
	- clash
```

### 救援模式密码找回

[Reset lost root password - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/Reset_lost_root_password#Using_the_debug_shell)



### FAQ

- 禁用声音模块

    ```shell
    rmmod pcspkr
    ```

- 移除无用软件包

    ```shell
    pacman -Rn $(pacman -Qdtq)
    ```

    

### 
