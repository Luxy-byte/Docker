# Docker网络

理解Docker0，在Linux宿主机上查看Docker0

```shell
[root@LuLuLuLuHost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000 # 本机回环地址
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000 #阿里云
    link/ether 00:16:3e:18:f7:c2 brd ff:ff:ff:ff:ff:ff
    inet 172.27.159.113/20 brd 172.27.159.255 scope global dynamic eth0
       valid_lft 314909094sec preferred_lft 314909094sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default # docker0
    link/ether 02:42:b2:6c:0d:07 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```



>问题：Docker如何处理容器间的网络访问

```shell
# 生成一个centos 7 容器 并查看内部网络设置ifconfig

[root@LuLuLuLuHost ~]# docker run -it --name mycentos -p 6666:6666 -v volumes01:/home/data centos:7 /bin/bash

[root@abb6eabb45e0 ~]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.7 # docker分配的网址  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:07  txqueuelen 0  (Ethernet)
        RX packets 1389  bytes 20367284 (19.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1233  bytes 74331 (72.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        
# 测试Linux宿主机 ping 容器内，可以联通
[root@LuLuLuLuHost ~]# ping 172.17.0.7
PING 172.17.0.7 (172.17.0.7) 56(84) bytes of data.
64 bytes from 172.17.0.7: icmp_seq=1 ttl=64 time=0.047 ms
64 bytes from 172.17.0.7: icmp_seq=2 ttl=64 time=0.056 ms
64 bytes from 172.17.0.7: icmp_seq=3 ttl=64 time=0.056 ms
64 bytes from 172.17.0.7: icmp_seq=4 ttl=64 time=0.059 ms
64 bytes from 172.17.0.7: icmp_seq=5 ttl=64 time=0.057 ms
^C
--- 172.17.0.7 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 3999ms
rtt min/avg/max/mdev = 0.047/0.055/0.059/0.004 ms

```

> 原理

1. 我们每启动一个docker容器，docker就会给docker容器分配一个IP，只要安装docker，就会存在一个网卡docker0，桥接模式，使用的技术是 evth-pairs（一对的虚拟设备接口，成对出现）

```shell
63: veth025abc8@if62: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default  # 62 -> 63
    link/ether 52:72:27:af:b6:be brd ff:ff:ff:ff:ff:ff link-netnsid 0
65: veth5ecd56d@if64: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default  # 64 -> 65
    link/ether f6:e3:22:a4:da:a8 brd ff:ff:ff:ff:ff:ff link-netnsid 2
73: vethbe6646d@if72: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default  # 72 -> 73
    link/ether 06:e1:97:bd:a2:ad brd ff:ff:ff:ff:ff:ff link-netnsid 1
```

结论 容器 和 容器之间可以互相 ping 通。

![Dokcer0网络](C:\Users\alienware\Desktop\Docker_Learn\Dokcer0网络.png)

**所有的容器在不指定网络的情况下，都是docker0路由的，docker会给我们的容器分配一个默认的 ip**，最多可分配大约65535个

删除以后 ip 就会被宿主机释放
