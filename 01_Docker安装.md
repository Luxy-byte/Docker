# Dock安装(centos7)

> 环境查看

```shell
#系统内核 3.10 以上
[root@LuLuLuLuHost lib]# uname -r
	3.10.0-957.21.3.el7.x86_64

```

```shell
#系统版本
[root@LuLuLuLuHost lib]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

```

> Docker安装

```shell
# 1.卸载旧版本Docker
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                  
# 2.环境安装包
yum install -y yum-utils

# 3.设置镜像仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo # 默认是从国外的
    #(建议替换:http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo)
    
# 更新yum软件包索引
yum makecache fast

# 4.安装dockers相关项 ce社区版 ee企业版 cli客户端 
yum install docker-ce docker-ce-cli containerd.io

# 5.启动docker
systemctl start docker

# 6.安装成功
docker version

# 7.测试hello world
docker run hello-world

# 8.查看hello world镜像
docker images
```

> Docker卸载

```shell
# 1.移除软件包
yum remove docker-ce docker-ce-cli containerd.io
# 2.删除根目录资源
rm -rf /var/lib/docker  #默认工作路径
rm -rf /var/lib/containerd
```



> 阿里云镜像加速

```shell
# 1.登录阿里云服务器官网
导航栏 - 容器镜像服务 - 镜像中心 - 镜像加速 选择 centos # 每个人的加速地址不一样
```

```shell
# 2.配置加速地址
1.sudo mkdir -p /etc/docker

2.sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://5jnz8p4y.mirror.aliyuncs.com"]
}
EOF

3.sudo systemctl daemon-reload

4.sudo systemctl restart docker
```



### 底层原理

> Docker是怎么工作的？

docker 是一个 cs 结果的系统，docker的守护进程运行在主机上，通过socket客户端访问！

dockerserver 接收到 dockerclient 的指令，就会执行这个指令

> Docker的优势

1.docker有着比虚拟机更少的抽象层

2.docker利用宿主机的内核

所以，新建容器的时候，docker不需要像虚拟机一样重新加载一个操作系统的内核，避免引导，虚拟机是加载Guest OS，分钟级别的，而docker是利用 宿主机的操作系统，省略了这个复杂的过程，秒级。

