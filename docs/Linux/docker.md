## Docker 指北
> docker 文档 [Reference documentation | Docker Documentation](https://docs.docker.com/reference/)

### Docker安装
Prerequisites

- A 64-bit installation
- Version 3.10 or higher of the Linux kernel. The latest version of the kernel available for your platform is recommended.
- `iptables` version 1.4 or higher
- `git` version 1.7 or higher
- ps命令支持
- [XZ Utils](https://tukaani.org/xz/) 4.9 or higher
- 整体的cgroups支持，不支持单一cgroups挂载点
	

安装

=== "官方仓库"
	- Docker 参考Tuna 基于的安装方式 [https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)	

=== " 二进制"
	Docker
	
	- Docker - [https://download.docker.com/linux/static/stable/](https://download.docker.com/linux/static/stable/)
	
	- Aliyun - [https://mirrors.aliyun.com/docker-ce/linux/static/stable/x86_64/](https://mirrors.aliyun.com/docker-ce/linux/static/stable/x86_64/)
	
		解压二进制包,拷贝至 `/usr/local/bin/` 目录中
	
		systemd 文件 [https://github.com/moby/moby/tree/master/contrib/init/systemd](https://github.com/moby/moby/tree/master/contrib/init/systemd) 拷贝至 `/usr/local/systemd/system` 文件夹中
	
	Docker-compose
	
	- Github [https://github.com/docker/compose/releases/](https://github.com/docker/compose/releases/)
	- Daocloud [http://get.daocloud.io/#install-compose](http://get.daocloud.io/#install-compose)

镜像加速
```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "debug": true,
  "data-root": "/home/docker",
  "registry-mirrors":["https://dh7sfazu.mirror.aliyuncs.com", "https://mirror.ccs.tencentyun.com"]
}
EOF
sudo systemctl reload docker
```

代理加速 / docker 认证
```shell
sudo mkdir -p ~/.docker
sudo tee ~/.docker/config.json <<-'EOF'
{	
    "auths": {
		"https://index.docker.io/v1/": {
			"auth": "" # token
		}
	},
	"proxies": {
		"default": {
			"httpProxy": "",
			"httpsProxy": "",
			"noProxy": "127.0.0.1/8,172.16.70.0/24"
		}
	}
}
EOF
sudo systemctl reload docker
```

### Usage 

- docker 配置 `~/.docker/config.json` [Use the Docker command line | Docker Documentation](https://docs.docker.com/engine/reference/commandline/cli/)
- docker-compose 配置 [Compose CLI environment variables | Docker Documentation](https://docs.docker.com/compose/reference/envvars/)
- dockerd 配置 `/etc/docker/daemon.json` [dockerd | Docker Documentation](https://docs.docker.com/engine/reference/commandline/dockerd) 官方参考[daemon.json](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file) 

| 描述                | 命令                                                | 备注 |
| ------------------- | --------------------------------------------------- | ---- |
| 删除docker None镜像 | `docker rmi $(docker images -f "dangling=true" -q)` |      |
| 删除docker stop容器 |                                                     |      |

### Tips
???+ tip "容器权限 privilege"
    Failed to setup loop device: No such file or directory 此报错为docker容器无法使用loop设备
    
	- mount: permission denied  此报错通常由于docker容器没有fs挂载权限导致
	- 解决： --privileged=true  使用超级模式运行

### Socket 配置指南

1. 配置tls证书 [Protect the Docker daemon socket | Docker Documentation](https://docs.docker.com/engine/security/protect-access/)
2. 开放socket

### Reference

[What even is a container: namespaces and cgroups (jvns.ca)](https://jvns.ca/blog/2016/10/10/what-even-is-a-container/)

