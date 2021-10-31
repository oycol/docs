## Onefs simulator

1. download the  ova image

2. unzip ova image

3. qemu-image cover vmdk to qcwo2

    ```shell
    for i in {1..n};do qemu-img convert 9.1.0.0-disk${i}.vmdk qcow2/9.1.0.0-disk${i}.qcow2 -O qcow2;done
    ```

4. import qcow image to new host

    ```shell
    for i in {1..n};do qm importdisk 103 qcow2/9.1.0.0-disk${i}.qcow2 ufs;done
    ```

5. attach the disk

    ```shell
    for i in {0..21};do qm set 103 --scsi${i} ufs:103/vm-103-disk-${i}.raw;done
    ```

```shell
for i in {1..22};do qemu-img convert 9.1.0.0-disk${i}.vmdk row/9.1.0.0-disk${i}.row ;done
for i in {1..22};do qm importdisk 104 row/9.1.0.0-disk${i}.row ufs;done
for i in {0..22};do qm set 105 --scsi${i} ufs:105/vm-104-disk-${i}.raw;done
```



创建pve虚拟机



| 步骤    | 操作                                           | 其他 |
| ------- | ---------------------------------------------- | ---- |
| general | 默认                                           |      |
| os      | don't use                                      |      |
| system  | bios: ovmf    /  uncheck add efi disk [option] |      |
| ...     | 默认                                           |      |
| network | vmware vmxnet3                                 |      |
|         |                                                |      |