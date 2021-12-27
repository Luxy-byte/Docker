# 部署Mysql

```shell
# 1.获取镜像 docker pull mysql:5.7

# 2.启动mysql容器
-d 后台运行
-p 端口映射
-v 数据挂载，多个-v多个目录挂载
-e 环境配置
[root@LuLuLuLuHost ~]# docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name='mysql01' mysql:5.7
882056d2747a0ccc821ed681700f4e43a9d34b4fa2df87b36b0f7480951e149c

# 3.启动成功之后，使用navicat 测试链接

# 4.容器删除 挂载到本地的数据卷 依旧没有丢失，实现数据持久化
```



## 具名和匿名挂载



```shell
# 匿名挂载
通过： docker run ......-v 容器内路径
-P 随机映射端口
docker run -d -P --name niginx01 -v /etc/nginx nginx

-v 只写了容器内路径，没有写容器外 路径

# 具名挂载
-v 卷名：容器内路径
docker run ......-v <名称>:/etc/nginx nginx

# 如何确定 具名挂载还是匿名挂载 还是指定路径挂载
-v 容器内路径          # 匿名挂载
-v 卷名:容器内路径      # 具名挂载
-v 宿主机路径:容器内路径 # 路径挂载

# 查看当前所有数据卷
docker volume ls

# 查看数据卷具体位置
docker volume inspect <卷名>
	# 所有的docker容器内的卷 没有指定目录的情况下都是保存在： /var/lib/docker/volumes/XXX/DATA....
	


# 括展
-v 容器内路径:ro rw改变读写权限
ro # 只读
rw # 读写

# 一旦设置了容器权限，容器对我们挂载出来的内容就有限定了
docker run -d .....-v <卷名>：/etc/nginx：ro nginx
docker run -d .....-v <卷名>：/etc/nginx：rw nginx

# ro 只能通过宿主机操作，容器内部无法操作
```

