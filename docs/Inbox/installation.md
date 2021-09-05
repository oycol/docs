## Containerd 部署

[Releases · containerd/containerd (github.com)](https://github.com/containerd/containerd/releases) 下载最新版本containerd

```shell
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# 设置必需的 sysctl 参数，这些参数在重新启动后仍然存在。
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

# 应用 sysctl 参数而无需重新启动
sudo sysctl --system


tar -C / -xf cri-containerd-cni*    --no-same-owner --no-overwrite-dir

# 生成默认配置文件
 mkdir /etc/containerd
 containerd config default > /etc/containerd/config.toml
```



## kubeadm 安装指南

> !!! tips 
>
> 关闭swap `swapoff -a`
>
> 开启ip转发 `/etc/sysctl.conf ` 关闭net.ipv4.ip_forward=1 注释 `sysctl -p`
>
> 安装conntrack `apt-get install conntrack`
>
> lsmod | grep br_netfilter
>
> 加载 netdilter 模块
>
> `sudo modprobe br_netfilter` 显式加载

1. iptable 桥接流量

   ```shell
   cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
   br_netfilter
   EOF
   
   cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
   net.bridge.bridge-nf-call-ip6tables = 1
   net.bridge.bridge-nf-call-iptables = 1
   EOF
   sudo sysctl --system
   
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

2. 使用systemd cgroups驱动

3. 

## 使用中遇到的问题

1. - scheduler            Unhealthy   Get "http://127.0.0.1:10251/healthz": dial tcp 127.0.0.1:10251: connect: connection refused   
     controller-manager   Unhealthy   Get "http://127.0.0.1:10252/healthz": dial tcp 127.0.0.1:10252: connect: connection refused 

     > 

2. - listing pod sandboxes: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService 

     > problem occurs at the place /etc/containerd/config.toml,
     > [plugins.cri.containerd]
     > snapshotter = "overlayfs" --> "native"
     > It works successfully, now!

