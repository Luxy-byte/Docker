# DockerFile指令

``` shell
# FROM     基础镜像，一切从这里开始构建

# MAINTAINER 镜像是由谁写的  姓名 + 邮箱 

# RUN  	   镜像构建的时候需要运行的命令

# ADD  	   步骤：nginx镜像，这个nginx镜像压缩包即为添加内容

# WORKDIR  镜像的工作目录 /bin/bash

# VOLUME   挂载的目录

# EXPOSE   暴露端口

# CMD 	   指定这个容器启动的时候要运行的命令  只有最后一个会生效，可被替代

# ENTRYPOINT 指定这个容器启动时候要运行的命令 可以追加命令

# ONBUILD  当构建一个被继承 DOCKERFILE 这个时候就会运行 ONBUILD 的指令。 触发指令。

# COPY     类似ADD 将我们文件拷贝到镜像中

# ENV      构建的时候设置环境变量
```

![DockerFile指令](C:\Users\alienware\Desktop\Docker_Learn\DockerFile指令.png)

> 创建自己的centos

```shell
# 编写DockerFile配置文件
[root@LuLuLuLuHost dockerfile]# cat mydockerfile 
FROM centos

MAINTAINER luxh<10333333@qq.com>

ENV MYPATH /usr/local

WORKDIR $MYPATH

RUN yum -y install vim

RUN yum -y install net-tools

EXPOSE 90

CMD echo $MYPATH

CMD echo "----end----"

CMD /bin/bash

# 构建镜像
# 命令：docker build -f dockerfile路径 -t 镜像名:[tag版本号]
[root@LuLuLuLuHost dockerfile]# docker build -f mydockerfile -t dfcentos:1.0 .
.
.
.
Successfully built a730f257a4ab
Successfully tagged dfcentos:1.0


# 测试  - 增加之后的镜像
[root@LuLuLuLuHost dockerfile]# docker run -it dfcentos:1.0
[root@0fdd623ac369 local]# pwd   # dockerfile 指定的工作目录
/usr/local
[root@0fdd623ac369 local]# ifconfig # dockerfile 新增的功能
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.5  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:05  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```

**docker history 查看镜像生成历史**

```shell
[root@LuLuLuLuHost dockerfile]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED             SIZE
dfcentos              1.0       a730f257a4ab   5 minutes ago       323MB
centosluxh            1.0       445088c8637c   About an hour ago   231MB
nginx                 latest    f6987c8d6ed5   2 days ago          141MB
mysql                 5.7       c20987f18b13   2 days ago          448MB
centos                latest    5d0da3dc9764   3 months ago        231MB
portainer/portainer   latest    580c0e4e98b0   9 months ago        79.1MB
[root@LuLuLuLuHost dockerfile]# docker history dfcentos:1.0  # 查看执行历史
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
a730f257a4ab   5 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B        
120ed0862913   5 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
8636d6fd5b17   5 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
1681083a6e82   5 minutes ago   /bin/sh -c #(nop)  EXPOSE 90                    0B        
7c099a9974d3   5 minutes ago   /bin/sh -c yum -y install net-tools             27.3MB    
3b4c63f21a8b   5 minutes ago   /bin/sh -c yum -y install vim                   64.7MB    
23e597dd4286   5 minutes ago   /bin/sh -c #(nop) WORKDIR /usr/local            0B        
8359e0a4ddcb   5 minutes ago   /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0B        
c9f1c352ecbd   5 minutes ago   /bin/sh -c #(nop)  MAINTAINER luxh<10333333@…   0B        
5d0da3dc9764   3 months ago    /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      3 months ago    /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
<missing>      3 months ago    /bin/sh -c #(nop) ADD file:805cb5e15fb6e0bb0…   231MB  
```

