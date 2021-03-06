## Netmaker  Openwrt  Support  Discussion

Hi, every one who use  netmaker, now there are some difficulties in openwrt adaptation, his ecology is so huge , so  I need your help to complete it.  

This is the table that  openwrt common CPU architecture,some architecture maybe have some trouble with running it. if you have some  good idea you can discuss with us，pull your request  maybe the best. 

| **ARCH** | Supported | Whether it is tested | CPU Model  Modal |
| -------- | --------- | -------------------- | ---------------- |
| amd64    | - [ ]     | - [ ]                |                  |
| arm      | - [ ]     | - [ ]                |                  |
| arm64    | - [ ]     | - [ ]                |                  |
| mips     | - [ ]     | - [ ]                |                  |
| mips64   | - [ ]     | - [ ]                |                  |
| mipsle   | - [ ]     | - [ ]                |                  |
| mips64le | - [ ]     | - [ ]                |                  |



### Netmaker  openwrt client usage

The netclient  should keep alive for get  all the client  info,  so it must run checkin for a period of time， in systemd， it was provided by timer service,but in openwrt ,there is no such mechanism,so , we create  a openwrt service to finsh it, on this way, you can move  netclient service on  /etcd/init.d/ directory, so you can run netclient service,  /etc/netclient/netclient binary is required, 

First , you should join server with parameter `--daemon off`, when you join  successful , move  openwrt-daemon.sh file  to /etcd/init.d/netclient,grant privilieges, start service and enable service,work completed. 

```shell
mv openwrt-daemon.sh /etcd/init.d/netclient
chomd a+x /etcd/init.d/netclient
service netclient start
service netclient enable
```

- start    start netclient service

- stop     stop  netclient service

- enable   startup on boot

    when you enable netclient service ，you can find it on  /etc/rc.d/



## Netmaker 使用指南

Obviously not

| device under test                                           | fireware                                                     | comment                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ----------------------- |
| [NEW_D2 （新三）](https://openwrt.org/toh/lenovo/newifi_d2) | [ ImmortalWrt 18.06](https://github.com/immortalwrt/immortalwrt/tree/openwrt-18.06) | Compatible with openwrt |
| Xiaomi CR6608                                               | [ ImmortalWrt 18.06](https://github.com/immortalwrt/immortalwrt/tree/openwrt-18.06) | Compatible with openwrt |
|                                                             |                                                              |                         |

### Netclient

openwrt  service

```shell
#!/bin/sh /etc/rc.common
# Example script
# Copyright (C) 2007 OpenWrt.org
 
start() {        
  bash /etc/netclient/checkin.sh &
}
 
stop() { 
  PID=`ps -ef|grep checkin.sh|grep -v grep|awk '{print $1}'`
  kill $PID
}
```



```shell
#!/bin/sh
while true;do
  ./netclient checkin -n all >> /tmp/checkin.log 2>&1
  LOG_SIZE=`ls -l /tmp/checkin.log|awk '{print $5}'`
  if [ $LOG_SIZE -qt 10485760 ];then
    mv /tmp/checkin.log /tmp/checkin.log.0
  fi
  sleep 10
done

```



### Troubling Shotting

- netmaker server无法自动加入(k3s ,containerd,nerdctl)

## NetMaker 网络质量稳定性测试

### 实验环境

| 设备     | ip状态 | 私有ip   | 操作系统    |
| -------- | ------ | -------- | ----------- |
| Netmaker | 公网   | 10.0.0.1 | ubuntu20.04 |
| cloud 1  | 公网   | 10.0.0.4 | archlinux   |
| laptop   | 无公网 | 10.0.0.2 | debian      |
|          | 无公网 |          | openwrt     |
|          |        |          |             |

## Mtr测试

- NetMaker线路

    | 源地址 | 目的地址 | 命令                          | Loss%   Snt   Last   Avg  Best  Wrst   StDev    |
    | ------ | -------- | ----------------------------- | ----------------------------------------------- |
    | laptop | cloud1   | mtr 10.0.0.4 -r -n -c 1000    | 0.2%    1000    9.1  10.5   8.7    20.5     0.6 |
    | laptop | cloud1   | mtr 10.0.0.4 -r -n -c 1000 -u | 0.0%    1000   10.7  10.5  8.7     19.0    0.6  |
    | laptop | cloud1   | mtr 10.0.0.4 -r -n -c 1000 -T |                                                 |

- 源线路

    | 源地址 | 目的地址 | 命令                               | Result |
    | ------ | -------- | ---------------------------------- | ------ |
    | laptop | cloud1   | mtr 123.207.27.32 -r -n -c 1000    |        |
    | laptop | cloud1   | mtr 123.207.27.32 -r -n -c 1000 -u |        |
    | laptop | cloud1   | mtr 123.207.27.32 -r -n -c 1000 -T |        |
    |        |          |                                    |        |
    
    - HOST: laptop                      Loss%   Snt   Last   Avg  Best  Wrst StDev 
          1.|-- 192.168.2.1               98.7%  1000    0.6   0.7   0.5   0.8   0.1 
          2.|-- 192.168.10.1              98.7%  1000    1.1   1.1   1.0   1.3   0.1 
          3.|-- 172.66.0.1                 0.0%  1000    5.8   6.5   2.8  48.2   4.8 
          4.|-- 183.233.67.101             0.0%  1000    6.5   6.8   4.2  49.4   2.8 
          5.|-- 120.196.242.157            0.0%  1000    8.2   7.4   5.4  52.1   1.9 
          6.|-- 120.196.199.110           98.2%  1000   15.5  15.5  15.1  17.2   0.5 
          7.|-- 120.241.50.2               0.1%  1000   17.3  17.5  16.2  36.0   0.8 
          8.|-- 10.162.32.146              0.0%  1000   14.4  14.2  12.9  37.4   1.1 
          9.|-- 10.196.18.78               8.7%  1000   19.2  20.4  18.0  32.6   2.7 
         10.|-- ???                       100.0  1000    0.0   0.0   0.0   0.0   0.0 
         11.|-- 9.31.123.3                 9.5%  1000   14.3  16.4  13.0 111.4   5.8 
         12.|-- 123.207.27.32              0.0%  1000   12.9  13.1  11.6  45.2   1.5 
    - N_HOST: laptop                      Loss%   Snt   Last   Avg  Best  Wrst StDev
          1.|-- 192.168.2.1                0.3%  1000    1.0   0.6   0.4  39.2   1.2
          2.|-- 192.168.10.1               0.1%  1000    5.9   5.6   1.4  44.0   1.6
          3.|-- 172.66.0.1                 0.0%  1000    9.3   9.6   4.9  52.6   2.1
          4.|-- 183.233.67.101             0.0%  1000   10.2  11.6   5.8  65.4   3.1
          5.|-- 120.196.242.157            0.0%  1000   11.7  12.1   7.5  56.6   2.3
          6.|-- 120.196.199.126           98.0%  1000   16.1  15.9  11.2  21.1   3.3
                120.196.199.110                  
          7.|-- 120.241.50.6               0.0%  1000   17.3  20.4  12.5  59.9   2.9
                120.241.50.10                    
                120.241.50.2                     
                120.241.50.14                    
          8.|-- 10.196.2.102               0.0%  1000   17.7  18.0  11.7  72.2   2.9
                10.196.93.229                    
                10.162.32.150                    
                10.162.32.146                    
          9.|-- 10.196.18.78              54.0%  1000   22.6  23.5  18.9  27.6   1.1
         10.|-- 10.196.18.126             28.9%  1000   18.3  20.1  11.8  28.3   2.8
                10.196.18.30                     
                10.196.18.110                    
                10.196.18.62                     
                10.196.18.78                     
                10.196.18.14                     
         11.|-- 9.31.123.1                31.2%  1000   18.6  20.9  13.4  63.8   4.2
                9.31.123.3                       
                9.31.123.97                      
                9.31.123.49                      
                9.31.122.209                     
                9.31.123.51                      
         12.|-- ???                       100.0  1000    0.0   0.0   0.0   0.0   0.0

## Iperf3 测试

client 发送数据给server端

- NetMaker线路

    | client | server | 命令                                          | Result                               |
    | ------ | ------ | --------------------------------------------- | ------------------------------------ |
    | laptop | cloud1 | iperf3 -c 10.0.0.4 --forceflush -Z  -t 75     | 6.00 Mbits/sec  608                  |
    | laptop | cloud1 | iperf3 -c 10.0.0.4 --forceflush -Z   -t 11 -u | 1.05 Mbits/sec  0.000 ms  0/1014 (0% |
    
- 源线路

    | client | server | 命令                                               | Result                               |
    | ------ | ------ | -------------------------------------------------- | ------------------------------------ |
    | laptop | cloud1 | iperf3 -c 123.207.27.32 --forceflush -Z  -t 75     | 30.6 Mbits/sec  6531                 |
    | laptop | cloud1 | iperf3 -c 123.207.27.32 --forceflush -Z   -t 20 -u | 1.05 Mbits/sec  0.000 ms  0/925 (0%) |

