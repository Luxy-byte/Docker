# Docker

docker是一个开源的应用容器引擎。

docker 三要素：镜像 、容器、仓库

docker官网：www.docker.com

docker中文网：www.docker-cn.com

docker-hub镜像管理平台：hub.docker.com

docker 利用容器独立运行的一个或一组应用。容器是用镜像创建的运行实列。

可以把容器看做是一个简易版的Linux系统(包括root用户权限，进程空间，用户空间，和 网络空间等)和运行在其中的应用程序

容器的定义和镜像几乎一摸一样，也是一堆层的统一视角，唯一的区别在于容器的最上面那一层是可读可写的。

p1 = person()



仓库：是集中存放镜像文件的场所

仓库和仓库注册服务器是有区别的。仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的tag标签

仓库分为公开仓库和私有仓库

最大的公开仓库是 Docker Hub

国内公开仓库包括阿里云，网易云等....



### docker 安装:

```dockerfile
yum update  -- yum包更新到最新
yum install -y yum-utils device-mapper-persistent-data lvm2  -- 安装需要的软件包
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo  -- 设置yum源
yum install -y docker-ce  安装社区版docker
docker -v  --查看docker版本，是否安装成功
```

-y 表示默认 yes。



### docker 架构：

- 镜像 - image：

  Docker镜像，就相当于是一个 root 文件系统。比如官方镜像 ubuntu18.04就包含了完整的一套ubuntu18.04最小系统的 root 文件系统

- 容器 - Container：

  镜像 和 容器 的关系，就像是面向对象程序设计中的类和对象一样，镜像是静态的定义，容器是镜像运行的实体。容器可以被 创建，启动，停止，删除，暂停等等.

  - 容器是完全使用沙箱机制，相互隔离
  - 容器性能开销极低
  - 基于Go语言实现

- 仓库 - Repository：

  仓库可以看成一个代码控制中心，用来保存镜像

![docker_架构](C:\Users\alienware\Desktop\Z_Code\docker_架构.png)

配置镜像加速器 -- 参照 阿里云服务官网

### docker 服务相关命令：

```dockerfile
systemctl start docker  -- 启动
systemctl status docker -- 停止
systemctl stop docker   -- 重启
systemctl restart docker-- 查看服务状态
systemctl enable docker -- 设置开机启动

docker version 查看docker版本
docker info  查看docker详细信息
docker --hlep  查看docker的帮助
```



### docker 镜像相关命令：

```dockerfile
docker images -- 查看当前主机的镜像
docker images -a 查看当前主机的全部镜像
docker images -q 查看当前主机的全部镜像ID
docker search <指定镜像>  -- 查看是否存在指定的镜像
docker pull <指定镜像>  -- 拉取指定镜像，默认 latest 的镜像
docker pull <指定镜像>:<对应版本号>  -- 拉去指定镜像的对应版本	
docker rmi <指定镜像> <对应镜像id>   -- 删除指定的镜像，如果存在两个同名镜像，通过id 删除指定的镜像
docker rmi <指定镜像>:<对应镜像版本号>   -- 删除指定的镜像，如果存在两个同名镜像，通过版本号 删除指定的镜像
docker rmi `docker images -q` -- 删除全部镜像
```

hub.docker.com 可以查看指定镜像的支持的版本。

### docker 容器相关命令 - 上：

```dockerfile
docker run -it --name=<your name> centos:7 /bin/bash   --创建容器

docker ps -- 查看当前正在运行的容器
docker ps -a -- 查看关闭和没有关闭的所有容器

docker exec -it <指定容器名称> /bin/bash  -- 进入正在运行的容器
```

-i : 保持容器运行,通常与-t同时使用，容器创建后自动进入容器中，推出容器后，容器自动关闭

-t : 给容器分配一个伪输入终端。

-d：以守护后台模式运行容器，需要docker exec进入容器。退出后，容器不会关闭

-it：交互式容器

-d：守护式容器

--name：给容器命名

/bin/bash ：进入容器自动进入中端指令操作模式



### docker 容器相关命令 - 下：

```dockerfile
docker stop <容器名称>  -- 停止指定容器运行
docker start <容器名称> -- 启动指定容器运行
docker rm <容器ID><容器名称> -- 通过ID或者名称 删除指定的 容器
docker ps -aq 查看所有容器的ID
docker rm `docker ps -aq` -- 删除所有容器
docker inspect <容器名称> -- 查看指定容器的信息

exit  -- 退出当前容器
docker attach <容器名称> -- 进入容器并进行交互模式
docker start <容器名称>  -- 启动容器
docker stop <容器名称> -- 停止容器

```

注意开启的容器，是不能删除的。



### docker 容器的数据卷：

- 数据卷概念及作用 

  - 数据卷是宿主机中的一个目录或文件

  - 当容器目录和数据卷目录绑定后，对方的修改会立即同步

  - 一个数据卷可以被多个容器同时挂载

  - 一个容器也可以 被 挂载多个数据卷

    ![docker_数据卷](C:\Users\alienware\Desktop\Z_Code\docker_数据卷.png)

- 配置数据卷

  ```dockerfile
  docker run ..... -v 宿主机目录(文件):容器内目录(文件) .....、
  
  列:
  挂载单一数据卷:
  docker run -it --name=c1 -v /root/data:/root/data_container centos:7 /bin/bash
  
  挂载多个数据卷:
  docker run -it --name=c2 -v ~/root/data2:/root/data2 ~/root/data3:/root/data3 centos:7
  
  
  
  ```

  注意事项：

  - 目录必须是绝对路径
  - 如果目录不存在，会自动创建
  - 可以挂载多个数据卷

- 配置数据卷容器

  - 多容器进行数据交换

    - 多个容器挂载同一个数据卷
    - 数据卷容器
    - ![docker_数据卷容器](C:\Users\alienware\Desktop\Z_Code\docker_数据卷容器.png)

    ```dockerfile
    列：
    1.创建启动 c3 数据卷容器，使用-V 参数 设置数据卷
    docker run -it --name=c3 -v /volume centos:7
    
    2.创建 c1 c2 容器，使用 --volumes-from 参数设置数据卷
    docker run -it --name=c1 --volumes-from c3 centos:7
    docker run -it --name=c2 -v --volumes-from c3 centos:7
    ```

    

### docker 部署 mysql:

**端口映射：**

- 容器内的网络服务和外部机器不能直接通信

- 外部机器和宿主机可以直接通信

- 宿主机和容器可以直接通信

- 当容器中的网络服务需要被外部机器访问时，可以将容器中提供服务的端口映射到宿主机端口上。外部机器访问宿主机的 该 端口，从而间接访问容器的服务。

  ```dockerfile
  docker search mysql		-- 查看镜像源是否存在mysql
  docker pull mysql:5.7   -- 拉取mysql5.7到本地
  
  mkdir ~/mysql  -- root 根目录下创建mysql文件夹 方便数据卷管理
  cd ~/mysql
  
  docker run -id -p 3307:3306 --name=t_mysql \
  -v ~/mysql/conf:/etc/mysql/conf.d \      -- 分别挂载mysql 配置文件conf.d logs mysql数据 到宿主机本地
  -v ~/mysql/logs:/logs \
  -v ~/mysql/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7  -- -e表示环境变量设置登录密码
  
  docker exec -it <容器名称: t_mysql> /bin/bash  -- 进入需要操作的容器 后缀最好用 /bin/bash
  ```

  

### docker 部署 Nginx:



