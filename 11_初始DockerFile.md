# 初始Docker File

Dockerfile就是用来构建 docker 镜像的构建文件！命令脚本！

通过脚本可以生成镜像，镜像是一层一层的，脚本一个一个的命令，每个命令就是一层

> 方式二：

```shell
# 创建一个dockerfile文件，名字可以随意。
# 文件内容 指令（大写） 参数（小写）
FROM centos

VOLUME ["volume01','volume02']  # 匿名挂载

CMD echo "----end----"
CMD /bin/bash
~   # 每个命令就是镜像的一层

# 构建dockerfile文件镜像
[root@LuLuLuLuHost docker-test-volume]# docker build -f /home/docker-test-volume/dockerfile01 -t centosluxh:1.0 . # 注意最后的点结尾
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 5d0da3dc9764
Step 2/4 : VOLUME ["volume01","volume02"]
 ---> Running in 5d7dd33c0892
Removing intermediate container 5d7dd33c0892
 ---> 4972ba31a864
Step 3/4 : CMD echo "----end----"
 ---> Running in 97d3e9cff5c1
Removing intermediate container 97d3e9cff5c1
 ---> 1bc002a8285f
Step 4/4 : CMD /bin/bash
 ---> Running in 6a1ce4fbe0f7
Removing intermediate container 6a1ce4fbe0f7
 ---> 445088c8637c
Successfully built 445088c8637c
Successfully tagged centosluxh:1.0
# 查看新生成的镜像
[root@LuLuLuLuHost docker-test-volume]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
centosluxh            1.0       445088c8637c   15 seconds ago   231MB
nginx                 latest    f6987c8d6ed5   2 days ago       141MB
mysql                 5.7       c20987f18b13   2 days ago       448MB
centos                latest    5d0da3dc9764   3 months ago     231MB
portainer/portainer   latest    580c0e4e98b0   9 months ago     79.1MB

# 启动镜像容器，使用id启动 而不是名字
[root@LuLuLuLuHost docker-test-volume]# docker run -it 445088c8637c /bin/bash
[root@152674677c9f /]# ls -l
total 56
lrwxrwxrwx  1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x  5 root root  360 Dec 23 17:39 dev
drwxr-xr-x  1 root root 4096 Dec 23 17:39 etc
drwxr-xr-x  2 root root 4096 Nov  3  2020 home
lrwxrwxrwx  1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx  1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------  2 root root 4096 Sep 15 14:17 lost+found
drwxr-xr-x  2 root root 4096 Nov  3  2020 media
drwxr-xr-x  2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x  2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 98 root root    0 Dec 23 17:39 proc
dr-xr-x---  2 root root 4096 Sep 15 14:17 root
drwxr-xr-x 11 root root 4096 Sep 15 14:17 run
lrwxrwxrwx  1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x  2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x 13 root root    0 Dec 22 10:14 sys
drwxrwxrwt  7 root root 4096 Sep 15 14:17 tmp
drwxr-xr-x 12 root root 4096 Sep 15 14:17 usr
drwxr-xr-x 20 root root 4096 Sep 15 14:17 var
drwxr-xr-x  2 root root 4096 Dec 23 17:39 volume01 # 这就是生成镜像时的数据卷目录
drwxr-xr-x  2 root root 4096 Dec 23 17:39 volume02 # 使用docker inspect 可以查看宿主机对应的位置

```

