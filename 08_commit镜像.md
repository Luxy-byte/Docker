# Commit镜像

```shell
# 命令：docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交的信息" -a='坐着' 容器id 目标镜像名:[tag]
```

## 测试：

```shell
# 1.创建一个新的centos容器，并创建一个luxh.txt文件
[root@LuLuLuLuHost run]# docker run --name='mycentos' -it -d centos
7555b40b35ea048db37440f10d5c09dbc891e33ea992b3aef09d508d303cbacf
[root@LuLuLuLuHost run]# docker ps -a
CONTAINER ID   IMAGE                 COMMAND         
7555b40b35ea   centos                "/bin/bash"       

# 3.将我创建luxh.txt的容器 打包为一个新的镜像
[root@LuLuLuLuHost run]# docker commit -m="我的centos" -a='lulu' 7555b40b35ea centosluxh:1.0
sha256:d18a2671e0f3a8f66d009219de8a278e77f3c24c473147dec4a97a3c29c43d3f

# 4.查看我们自己生成的镜像
[root@LuLuLuLuHost run]# docker images -a
REPOSITORY            TAG       IMAGE ID       CREATED         SIZE
centosluxh            1.0       d18a2671e0f3   5 seconds ago   231MB
```

