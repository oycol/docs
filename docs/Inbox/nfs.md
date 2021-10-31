## NFS SERVER

> [NFSServerSetup - Debian Wiki](https://wiki.debian.org/NFSServerSetup)

### Requirement 

- kernel 级别支持` knfsd.ko`  

    ```shell
    grep NFSD /boot/config-`uname -r`
    ```
    或者配置文件的自定义存放位置



### implementation

- User Space - (slower, easier to debug)
- Kernel Space - (faster)

### Installation

1.  ```shell
      apt install nfs-kernel-server portmap
     ```

    `portmap` package is only required if you want to run an NFSv2 or NFSv3 server

2. 



  

