

## 系统架构

概念 [5.1 Kubernetes介绍 · Docker和Kubernetes实践指南 (unixhot.com)](http://k8s.unixhot.com/kubernetes/kubernetes-introduce.html#system)

- **API Server**：提供Kubernetes API接口，主要处理 Rest操作以及更新Etcd中的对象。是所有资源增删改查的唯一入口。
- **Scheduler**：绑定Pod到Node上，主要做资源调度。
- **Controller Manager**：所有其他群集级别的功能，目前由控制器Manager执行。资源对象的自动化控制中心，Kubernetes集群有很多控制器。
- **Etcd**：所有持久化的状态信息存储在Etcd中，这个是Kubernetes集群的数据库。

## 逻辑架构

- **Pod**

  Pod是Kubernetes的最小管理单元，一个Pod可以包含一组容器和卷。虽然一个Pod里面可以包含一个或者多个容器，但是Pod只有一个IP地址，而且Pod重启后，IP地址会发生变化。

- **Controller**

  一个应用如果可以有一个或者多个Pod，就像你给某一个应用做集群，集群中的所有Pod是一模一样的。Kubernetes中有很多控制器可以来管理Pod，例如下图的Replication Controller可以控制Pod的副本数量，从而实现横向扩展。

  - Replication Controller（新版本已经被ReplicaSet所替代）
  - ReplicaSet（新版本被封装在Deployment中）
  - Deployment：封装了Pod的副本管理、部署更新、回滚、扩容、缩容等功能。 无状态应用，管理的所有的pod一样，提供同一个服务，不需要考虑在那台Node运行，pod使用同一个数据卷，对外提供统一的服务。

  - DaemonSet：保证所有的Node上有且只有一个Pod在运行。

  - StatefulSet：有状态的应用，为 Pod 提供唯一的标识，它可以保证部署和 scale 的顺序。独享存储

      分布式应用部署实例是会有依赖关系，主备关系，被称为有状态，例如MySql主从，etcd集群

      1. 固定主机名，专有存储卷
      2. pod有序的部署、扩容、删除、停止
      3. pod分配稳定、唯一的网络标识
      4. 独享存储 

  - Job：使用Kubernetes运行单一任务。 批量处理的任务，达到指定的次数时，任务完成就完成了，删除job将清除其创建的Pod。

  - CronJob：使用Kubernetes运行定时任务。

- **Service**

  由于Pod的生命周期是短暂的，而且每次重启Pod的IP地址都会发生变化，而且一个Pod有多个副本，也就是说一个集群中有了多个节点，就需要考虑负载均衡的问题。Kubernetes使用Service来实现Pod的访问，而且Service有一个Cluster IP，通常也称之为VIP，是固定不变的。
  
  4 层代理
  
  ingress 7 层代理

## 网络

在Kubernetes集群中存在着三种网络，分别是Node网络、Pod网络和Service网络，这几种网络之间的通信需要依靠网络插件，Kubernetes本身并没有提供，社区提供了像Flannel、Calico、Cannal等，后面章节会详述。

**Node网络**

Node网络指的是Kubernetes Node节点本地的网络，在本实验环境中使用的是192.168.56.0/24这个网段，所有的Node和Master在该网段都可以正常通信。

```shell
kubectl get pod -o wide
```



**Pod网络**

后面创建的Pod，每一个Pod都会有一个IP地址，这个IP地址网络段被称之为Pod网络

**Service网络**

Service是为Pod提供访问和负载均衡的网络地址段

```shell
kubectl get service
```



## 安装

## yaml文件

k8s api对象的定义有两个部分

- Metadata 部分，存放对象的元数据

- Spec部分，对象独有的定义，描述对象表达的功能

    selector

    控制器定义 ↑

    ---

    被控制对象 ↓

    template： Pod 模板 

## 网络

学到k8s 这块,发现主要的问题还是在

### Service

### Ingress 

关于

### MetaILB

安装：

 
