# 安装Docker-Nginx

> Docker部署Nginx

```shell
# 1. 搜索镜像 search 建议docker hub搜索，可以看到详细信息和帮助文档
# 2. 下载镜像 pull
# 3. 运行测试
[root@LuLuLuLuHost home]# docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    f6987c8d6ed5   38 hours ago   141MB
    # -d 后台运行
    # --name 给容器起名
    # -p 宿主机端口:容器内部端口
[root@LuLuLuLuHost home]# docker run -d --name='nginx01' -p 3344:80 nginx
f13ab24712744b50ad779a91ab9d51b687a2b02e92c0a9d2a8f3b6215f779917
[root@LuLuLuLuHost home]# curl localhost:3344   # 查看测试端口
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

# 4.进入容器
[root@LuLuLuLuHost home]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS                  NAMES
f13ab2471274   nginx     "/docker-entrypoint.…"   8 minutes ago    Up 8 minutes                0.0.0.0:3344->80/tcp   nginx01
[root@LuLuLuLuHost home]# docker exec -it nginx01 /bin/bash
root@f13ab2471274:/# ls
bin   dev		   docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
....
root@f13ab2471274:/# cd /etc/nginx
root@f13ab2471274:/etc/nginx# ls
conf.d	fastcgi_params	mime.types  modules  nginx.conf  scgi_params  uwsgi_params

```

