ubuntu 发现很多cron文件

找到一些无用的配置

```
e2scrub_all 检查lvm磁盘（ext）文件格式
popularity-contest 发送软件包的使用情况
apport 删除核心转储
apt-compat 系统更新
```

- 笔记本关盖不休眠

```shell
vi /etc/systemd/logind.conf
# 把 HandleLidSwitch 的参数改为 lock, 并去掉前面的注释
systemctl restart systemd-logind
```

```shell
# 制作虚拟机模板
#!/bin/bash

VM_NAME=focal-server-template
VM_ID=9000
VM_CORES=4
VM_MEM=4096
VM_IMAGE=focal-server-cloudimg-amd64.img
VM_DISK=40G
VM_STORAGE=local

qm create $VM_ID --cores $VM_CORES --memory $VM_MEM --name $VM_NAME --net0 virtio,bridge=vmbr0 # 设置os type

# 导入下载的镜像到local-lvm 存储空间
qm importdisk $VM_ID $VM_IMAGE $VM_STORAGE

# 将导入的磁盘以 scsi 方式挂载到虚拟机上面
qm set $VM_ID --scsihw virtio-scsi-pci --scsi0 $VM_STORAGE:$VM_ID:vm-$VM_ID-disk-0.raw

# 添加 Cloud-Init CDROM 驱动（必须添加这个vm才能启动cloud-init）
qm set $VM_ID --ide2 $VM_STORAGE:cloudinit

# resize 磁盘
qm resize $VM_ID scsi0 $VM_DISK

# 设置启动
qm set $VM_ID --boot c --bootdisk scsi0

# 
timedatectl set-timezone Asia/Shanghai
# 镜像源
cat << EOF > /etc/apt/sources.list 
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
EOF

sed -i "s/- apt-configure/#- apt-configure/g" /etc/cloud/cloud.cfg

truncate -s0 /etc/hostname

systemctl mask apt-daily.service apt-daily-upgrade.service


cloud-init clean && rm -rf /var/lib/cloud/*

echo -n > /etc/machine-id

apt clean cat /dev/null > ~/.bash_history && history -c history -w


```



- Pve 排错原来发现是sbin 是个链接文件，被我覆盖了

  resize2fs /dev/mapper/pve-root



pve 单cluster 节点无法启动, 参考文档 [[SOLVED\] - Another "cluster not ready - no quorum? (500)" case | Proxmox Support Forum](https://forum.proxmox.com/threads/another-cluster-not-ready-no-quorum-500-case.56104/)

```shell
pvecm expected 1
```



## 手动安装pve

修改hosts

```shell
echo -e "127.0.0.1 localhost \n192.168.2.30 pve" > /etc/hosts
echo "deb [arch=amd64] http://download.proxmox.com/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
wget https://enterprise.proxmox.com/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg 
apt update && apt full-upgrade
apt install proxmox-ve postfix open-iscsi
apt remove os-prober

```
