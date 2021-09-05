## SSH 端口转发

### 本地端口转发

![img](images/733013-20170706232654237-1722009435.png)

>   将本地端口绑定到目标主机的端口

```shell
# host1 执行
ssh -g -L 本地端口:目标主机:目标主机端口 跳板机  / -g 允许外部连接（否则只能自身访问） -f 以后台方式执行端口转发 / 最佳实践 ssh -f -N -g -L 
```

### 远程端口转发 - MARKED

>   还没搞懂 [SSH隧道：端口转发功能详解 - 骏马金龙 - 博客园 (cnblogs.com)](https://www.cnblogs.com/f-ck-need-u/p/10482832.html)

![img](images/733013-20170706232859284-1595079781.png)

```shell
# host3 执行
ssh -R [bind_addr:]host1_port:host2:port2 host1
```

### 动态端口转发(SOCKS代理)

![img](images/733013-20170706233246425-1384840260.png)

```shell
# host1 执行
ssh -D [bind_addr:]port remote 
ssh -Nfg -D 2222 host3
# host1 上产生的数据都会由2222端口转发给host3
```