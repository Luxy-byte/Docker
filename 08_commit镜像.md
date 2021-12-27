# Commit镜像

```shell
# 命令：docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交的信息" -a='作者' 容器id 目标镜像名:[tag]
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



> 镜像打包

```shell
# 镜像打包
docker save -o /root/xxx.tar <name>

[root@LuLuLuLuHost ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
luluredis             1.0       4da22cef58b7   52 minutes ago   113MB
redis                 latest    7614ae9453d1   6 days ago       113MB

[root@LuLuLuLuHost ~]# docker save -o /root/luredis.tar 4da22cef58b7
[root@LuLuLuLuHost ~]# ls
luredis.tar

# 镜像导入
docker load -i /root/xxx.tar

[root@LuLuLuLuHost ~]# docker load -i luredis.tar 
6a113940bec0: Loading layer [==================================================>]  4.608kB/4.608kB
Loaded image ID: sha256:4da22cef58b76f3a05764c9489d0165f47b2528e5bb3ed32c5d25d1064fb9721
[root@LuLuLuLuHost ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
<none>                <none>    4da22cef58b7   59 minutes ago   113MB # docker tag 可以重新替换名称
redis                 latest    7614ae9453d1   6 days ago       113MB

# 容器打包
docker export -o /root/xxx.tar <name>

# 容器导入
docker import xx.tar <name>:[tag]
```

