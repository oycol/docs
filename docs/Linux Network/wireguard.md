# wireguard指北

## 安装

1.  Installation

```shell
# Ubuntu ≥ 18.04
apt install wireguard

# Centos
yum install epel-release https://www.elrepo.org/elrepo-release7.el7.elrepo.noarch.rpm
yum install yum-plugin-elrepo
yum install kmod-wireguard wireguard-tools

# Windows
`https://download.wireguard.com/windows-client/`

# Macos
brew install wireguard-tools
```

2.  中继服务器

```shell
# 开启转发
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
echo "net.ipv4.conf.all.proxy_arp = 1" >> /etc/sysctl.conf
sysctl -p /etc/sysctl.conf

# 允许本机 Net转换
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i wg0 -o wg0 -m conntrack --ctstate NEW -j ACCEPT
iptables -t nat -A POSTROUTING -s 10.10.2.0/24 -o eth0 -j MASQUERADE

# net 换成需要设置的内网net /  eth0 换成机器上的网口名称
```

3.  配置文件

    配置文件可以放在任何路径下，但必须通过绝对路径引用。默认路径是 `/etc/wireguard/wg0.conf`

4.  生成密钥

    ```shell
    #生成私钥/公钥
    wg genkey | tee wg.key | wg pubkey > wg.pub
    ```
    
5.  启动停止

    ```shell
    wg-quick up /full/path/to/wg0.conf
    wg-quick down /full/path/to/wg0.conf
    ```

## 命令行接口

```shell
# 启动/停止 VPN 网络接口
ip link set wg0 up
ip link set wg0 down

# 注册/注销 VPN 网络接口
ip link add dev wg0 type wireguard
ip link delete dev wg0

# 注册/注销 本地 VPN 地址
ip address add dev wg0 10.10.1.1/32
ip address delete dev wg0 10.10.1.1/32

# 添加/删除 VPN 路由
ip route add 10.10.1.1/32 dev wg0
ip route delete 10.10.1.1/32 dev wg0


# 查看系统 VPN 接口信息
ip link show wg0

# 查看 VPN 接口详细信息
wg show all
wg show wg0


# 查看系统路由表
ip route show table main
ip route show table local

# 获取到特定 IP 的路由
ip route get 10.10.0.1	
```

## 配置文件

>   默认路径是 `/etc/wireguard/wg0.conf`。
>
>   配置文件的命名形式必须为 `${WireGuard 接口的名称}.conf`。通常情况下 WireGuard 接口名称以 `wg` 为前缀，并从 `0` 开始编号，但你也可以使用其他名称，只要符合正则表达式 `^[a-zA-Z0-9_=+.-]{1,15}$` 就行。



```ini
# 此配置文件表明本地作为Server
[Interface]
Address = 10.10.1.1/32
ListenPort = 51820
PrivateKey = local_PrivateKey=
DNS = 1.1.1.1,8.8.8.8
Table = 12345
MTU = 1500
PreUp = /bin/example arg1 arg2 %i
PostUp = /bin/example arg1 arg2 %i
PreDown = /bin/example arg1 arg2 %i
PostDown = /bin/example arg1 arg2 %i

[Peer]
AllowedIPs = 10.10.1.0/24
Endpoint = node1.example.tld:51820
PublicKey = remote_PublicKey=
PersistentKeepalive = 25	

```

### [Interface]  - 本地配置

-   本地作为客户端,只路由自身流量，暴露一个ip

```ini
[Interface]
# Name = phone.example-vpn.dev
Address = 10.10.1.1/32
PrivateKey = <private key for phone.example-vpn.dev>
``````````

-   本地作为中继，公开整个vpn子网路由
```ini
[Interface]
# Name = public-server1.example-vpn.tld
Address = 10.10.1.0/24
ListenPort = 51820
PrivateKey = <private key for public-server1.example-vpn.tld>
DNS = 1.1.1.1
```

1.  Name  - 标识该配置属于哪个节点
2.  Address - 定义本地节点对哪个地址路由 （CIDR）
3.  ListenPort - 中继节点指定该参数监听vpn连接接口，默认端口号是 `51820`
4.  PrivateKey - 私钥 所有节点都必须设置，不可与其他服务器公用，`wg genkey > example.key`
5.  DNS - 客户端使用指定的DNS来处理VPN子网中的DNS请求
6.  Table - 定义子网使用的路由表，默认不需要设置
7.  Mtu - 默认
8.  PreUp - 启动vpn接口之前运行的命令
9.  PostUp - 启动vpn接口之后运行的命令
10.  PreDown - 停止vpn接口之前运行的命令
11.  PostDown - 停止vpn接口之后运行的命令



### [Peer] - 对等客户端

>   描述： 对等节点（peer）可以是将流量转发到其他对等节点（peer）的中继服务器
>
>   定义能够为一个或多个地址路由流量的对等节点（peer）的 VPN 设置。通过公网或内网直连的客户端。
>
>   中继服务器必须将所有的客户端定义为对等节点（peer），除了中继服务器之外，其他客户端都不能将位于 NAT 后面的节点定义为对等节点（peer），因为路由不可达。对于那些只为自己路由流量的客户端，只需将中继服务器作为对等节点（peer），以及其他可以直接访问的节点。



1.  Endpoint - 指定远端对等节点的公网地址

    ```ini
    Endpoint = ip/domain:port(51820)
    ```

2.  AllowedIPs - 允许该对等节点（peer）接收的 VPN 流量中的源地址范围

    -   客户端 	只路由自身的流量

        ```ini
        AllowedIPs = 10.10.1.1/32
        ```

    -   中继节点 - 可以将流量转发给其他对等节点
    
        ```ini
        AllowedIPs = 10.10.1.0/24
        ```
    
    -   中继服务器 - 可以转发所有的流量，包括外网流量和VPN流量
    
        ```ini
        AllowedIPs = 0.0.0.0/0,::/0
        ```
    
        即便只转发 `IPv4` 流量，也要指定一个 `IPv6` 网段，以避免将 `IPv6` 数据包泄露到 VPN 之外。详情参考：[reddit.com/r/WireGuard/comments/b0m5g2/ipv6_leaks_psa_for_anyone_here_using_wireguard_to](https://www.reddit.com/r/WireGuard/comments/b0m5g2/ipv6_leaks_psa_for_anyone_here_using_wireguard_to/)
    
    -   中继服务器 - 可以路由其自身和其他对等节点（peer）的流量
    
        ```ini
        AllowedIPs = 10.10.1.1/32,10.10.1.2/32
        ```
        
    -   中继服务器 - 可以路由其自身的流量和它所在的内网的流量
    
        ```ini
        AllowedIPs = 10.10.1.1/32,10.10.1.0/24
        ```
    
3.  PublicKey - 对等节点（peer）的公钥，所有节点（包括中继服务器）都必须设置。可与其他对等节点（peer）共用同一个公钥

4.  PersistentKeepalive

    >   如果连接是从一个位于 NAT 后面的对等节点（peer）到一个公网可达的对等节点（peer），那么 NAT 后面的对等节点（peer）必须定期发送一个出站 ping 包来检查连通性，如果 IP 有变化，就会自动更新`Endpoint`

    -   本地节点与对等节点可直连，不需要检查
    -   对等节点位于NAT后，该字段不需要指定，因为维持连接是客户端（连接的发起方）的责任
    -   本地节点位于NAT后，对等节点（peer）公网可达，需要指定该字段 `PersistentKeepalive = 25`，表示每隔 `25` 秒发送一次 ping 来检查连接

## [NAT TO NAT]

-   [ ] TODO

### Web 管理

```bash
# 重载wg配置文件
# /usr/lib/systemd/system/wg-quick@.service
[Unit]
Description=WireGuard via wg-quick(8) for %I
After=network-online.target nss-lookup.target
Wants=network-online.target nss-lookup.target
PartOf=wg-quick.target
Documentation=man:wg-quick(8)
Documentation=man:wg(8)
Documentation=https://www.wireguard.com/
Documentation=https://www.wireguard.com/quickstart/
Documentation=https://git.zx2c4.com/wireguard-tools/about/src/man/wg-quick.8
Documentation=https://git.zx2c4.com/wireguard-tools/about/src/man/wg.8

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/wg-quick up %i
ExecStop=/usr/bin/wg-quick down %i
ExecReload=/bin/bash -c 'exec /usr/bin/wg syncconf %i <(exec /usr/bin/wg-quick strip %i)'
Environment=WG_ENDPOINT_RESOLUTION_RETRIES=infinity

[Install]
WantedBy=multi-user.target
```



## 手动设置wireguard

添加wireguard接口

```shell
ip link add dev wg0 type wireguard
```

给wg0接口添加ip

```shell
ip address add dev wg0 10.10.2.1/24
```

 启动wireguard

```shell
ip link set up dev wg0
```