### 构建Centos7 Iso的原理

版本发行工具

```shell
yum -y install createrepo mkisofs isomd5sum rsync
```

### RPMBUILD

-   打包不打debuginfo   `echo '%debug_package %{nil}' >> ~/.rpmmacros`

### 同步iso目录

```shell
/usr/bin/rsync -a --exclude=Packages/ --exclude=repodata/ /mnt .
```

### 生成rpm repodata

```shell
createrepo -g comps.xml ./
```

### 构建镜像

```shell
genisoimage -joliet-long -V centos -o /home/QIKA.iso  -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -cache-inodes -T -eltorito-alt-boot -e images/efiboot.img -no-emul-boot ./
```

