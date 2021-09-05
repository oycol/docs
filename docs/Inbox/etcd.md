## Etcd 概念理解

### leases 租约

>   创建租约 并给予生命周期 `etcdctl lease grant 120`
>
>   查看租约列表 `etcdctl lease list`

用租约id创建的key生命周期跟随该 lease

续约 `etcdctl lease keep-alive 018f6d7bd032c117` 命令不会结束，会自动续约

## Cluster

两个步骤重新配置集群

1.  Inform cluster of new configuration
    
    >   To add a member into an etcd cluster, make an API call to request a new  member to be added to the cluster. This is the only way to add a new  member into an existing cluster. The API call returns when the cluster  agrees on the configuration change.

2.  Start new member

    >   To join the new etcd member into the existing cluster, specify the correct `initial-cluster` and set `initial-cluster-state` to `existing`. When the member starts, it will contact the existing cluster first and  verify the current cluster configuration matches the expected one  specified in `initial-cluster`. When the new member successfully starts, the cluster has reached the expected configuration.

    
    

???+ error "Do not use public discovery service for runtime reconfiguration"
    The public discovery service should only be used for bootstrapping a cluster. To join member into an existing cluster, use the runtime reconfiguration API.

## Discover

``` shell
http://172.16.128.13/v2/keys/_etcd/registry/17fad13-6ee2-4d2a-8de7-1305d4c6d3e2/_config/size -d value=3
```



## 参数说明

```json
# Member flags
–name 	 名称
–data-dir	数据目录
–listen-peer-urls	接受如下接口 (IP:port) 的http请求 (etcd间数据交互url)	default: “http://localhost:2380”
–listen-client-urls  客户端与etcd交互接口	default: “http://localhost:2379”

# Clustering flags
-initial-advertise-peer-urls  向其他cluster通知这个url	default: “http://localhost:2380”
-advertise-client-urls  向本集群其他成员通知成员的url default: “http://localhost:2379”

```





```shell
/bin/etcd --name etcd1 --listen-client-urls http://0.0.0.0:2379 --advertise-client-urls http://0.0.0.0:2379 --listen-peer-urls http://0.0.0.0:2380 --initial-advertise-peer-urls http://0.0.0.0:2380 --initial-cluster-token etcd-cluster-1 --initial-cluster-state new --enable-pprof --logger=zap --log-outputs=stderr
```

```shell
/bin/etcd --name etcd2 --listen-client-urls http://0.0.0.0:22379 --advertise-client-urls http://0.0.0.0:22379 --listen-peer-urls http://0.0.0.0:22380 --initial-advertise-peer-urls http://0.0.0.0:22380 --initial-cluster-token etcd-cluster-2 --initial-cluster-state new --enable-pprof --logger=zap --log-outputs=stderr
```


```shell
/bin/etcd --name etcd3 --listen-client-urls http://0.0.0.0:32379 --advertise-client-urls http://0.0.0.0:32379 --listen-peer-urls http://0.0.0.0:32380 --initial-advertise-peer-urls http://0.0.0.0:32380 --initial-cluster-token etcd-cluster-3 --initial-cluster-state new --enable-pprof --logger=zap --log-outputs=stderr
```