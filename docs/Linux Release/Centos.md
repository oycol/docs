###  Centos7.6 忘记密码恢复流程

1. 恢复模式内核 press e

2. **linux16** line and remove **rhgb,** **quiet** and **LANG** parameters,add **rd.break** at the end of line

3. ctrl+x 进入内核

4. 挂载系统 mount -o remount,rw  /sysroot

5. 切换root , chroot /sysroot

6. 修改密码 passwd

7. Optional - Set SELinux relabeling on next boot ( if selinux)

    ```
    # touch /.autorelabel
    ```

## 

- Centos7 永久路由

    根据接口创建路由配置文件/etc/syconfig/network-scripts/route-name，name表明接口dev名称

配置永久路由时，需要注意几点：

(1).**route-ethX的对应网卡配置文件ifcfg-ethX必须存在，否则路由无效。**(对于虚拟机，通常新添加的网卡都没有对应的ifcfg-ethX文件，但ifconfig却能找到该网卡)

(2).**如果在文件中配置永久默认路由，则必须保证所有使用了DHCP服务的网卡配置文件ifcfg-ethX中的DEFROUTE指令设置为"no"，表示DHCP不设置默认路由。**

(3).**如果在route-ethX文件中配置永久路由，且该网卡使用了DHCP服务分配地址，则必须保证该网卡的ifcfg-ethX文件中的PEERROUTES指令设置为"no"，表示DHCP设置的路由允许被覆盖。**
