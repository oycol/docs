## 使用kube部署kubenetes集群

### 初始化集群环境

| ip           | 主机名   | 角色   |
| ------------ | -------- | ------ |
| 192.168.2.26 | master26 | master |
| 192.168.2.27 | master27 | master |
| 192.168.2.28 | node28   | node   |

网络： 桥接

硬盘：40g

目的： 高可用apiserver，

### kubeadm安装和二进制安装

- kubeadm： kubeadm安装的组件都以pod运行，做到故障自恢复
- 二进制： systemd 维护，适用于理解k8s原理以及调试，不适合经常安装

### 前提条件

配置不同主机之间的hosts ，域名，时间

配置不同主机之间的无密钥登陆

关闭防火请

关闭selinux

关闭swap

cpu >=2

内核参数配置

配置ipvs  && 停用iptables [可选 ]  

安装containerd



### 集群证书

[PKI 证书和要求 | Kubernetes](https://kubernetes.io/zh/docs/setup/best-practices/certificates/)

集群默认生成一年证书，进行tls安全认证

Kubernetes 需要 PKI 才能执行以下操作：

- Kubelet 的客户端证书，用于 API 服务器身份验证
- API 服务器端点的证书
- 集群管理员的客户端证书，用于 API 服务器身份认证
- API 服务器的客户端证书，用于和 Kubelet 的会话
- API 服务器的客户端证书，用于和 etcd 的会话
- 控制器管理器的客户端证书/kubeconfig，用于和 API 服务器的会话
- 调度器的客户端证书/kubeconfig，用于和 API 服务器的会话
- [前端代理](https://kubernetes.io/zh/docs/tasks/extend-kubernetes/configure-aggregation-layer/) 的客户端及服务端证书

**说明：** 只有当你运行 kube-proxy 并要支持 [扩展 API 服务器](https://kubernetes.io/zh/docs/tasks/extend-kubernetes/setup-extension-api-server/) 时，才需要 `front-proxy` 证书

etcd 还实现了双向 TLS 来对客户端和对其他对等节点进行身份验证

### 安装K8s 

执行安装脚本

创建初始化文件

```shell
 kubeadm config print init-defaults > kubeadm-init.yaml
```

修改配置

```shell
kubeadm init --config kubeadm-init.yaml
```

忽略报错 `--ignore-preflight-errors=...`

创建.kube文件

安装网络组件[????]

初始化master 

- 根据file初始化

- 直接初始化

    ```shell
      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config
    
      export KUBECONFIG=/etc/kubernetes/admin.conf
    
    ```

    



### 将master加入集群

> `kubeadm token create --print-join-command`

1. 拷贝指定的证书
2. `kubeadm join ... --control-plane `

### 网络测试

创建pod 访问正常网络

暴露指定服务

`kubectl expose deployment nginx-deployment --port=80 --type=NodePort`

busybox 正常访问集群以及 nslookup正常解析

### 重置K8s

kubeadm reset

### 错误排查

- 集群状态NotReady ，网络组件没有启动
- crictl  创建pod 报错  删除containerd 配置文件后执行成功
- 使用的cgroups 驱动而 containerd 使用的 systemd 驱动，将两者驱动统一即可？？？

## 网络组件

- calico
- overlay

网络策略

## Kubectl

- 打标签  kubectl  label node node28 node-role.kubernetes.io/worker=worker

### 测试网络联通性

