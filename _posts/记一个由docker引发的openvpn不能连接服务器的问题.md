---
title: 记一个由docker引发的openvpn不能连接服务器的问题
date: 2017-09-07 16:59:33
tags: 
  - ubuntu
  - docker
  - openvpn
categories: 
  - 工具
---
## 由来
这几天同事在我们的服务器上搭建了openvpn来增强系统的安全性，并给我创建了一个证书。

我在可以连接上openvpn，但死活连不了我们服务器，我们一开始都怀疑是证书本身的问题。

## 定位
我们马上找了台windows机器测试，完全正常，没有任何问题。

完了，看来是我的非主流ubuntu系统的问题了。

正好，手边有另一套系统的openvpn，登录，一切正常。

难道说，是制作openvpn证书时，要增加什么特殊命令吗？

打开两个证书，对比了一下，并没有什么特殊之处。

完了，遇到迷之问题了。

准备洗洗睡了。。。。

## 灵感
在查找问题的时候我忽略了一个问题，就是：
- 我们服务器地址为 172.17.110.110
- 我可以ping通172.17.0.1
- 但我ping172.17.110.110时报host unreachable

这是一个非常重要的信息，但我就是没有get到任何东西，直到我敲出一下命令`ifconfig`
```
docker0   Link encap:以太网  硬件地址 02:42:02:7b:ee:bd  
          inet 地址:172.17.0.1  广播:0.0.0.0  掩码:255.255.255.0
          UP BROADCAST MULTICAST  MTU:1500  跃点数:1
          接收数据包:0 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:0 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:0 
          接收字节:0 (0.0 B)  发送字节:0 (0.0 B)

lo        Link encap:本地环回  
          inet 地址:127.0.0.1  掩码:255.0.0.0
          inet6 地址: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  跃点数:1
          接收数据包:301877 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:301877 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1000 
          接收字节:30098878 (30.0 MB)  发送字节:30098878 (30.0 MB)

wlp3s0    Link encap:以太网  硬件地址 84:3a:4b:72:c3:a8  
          inet 地址:192.168.1.101  广播:192.168.1.255  掩码:255.255.255.0
          inet6 地址: fe80::65ab:bf3e:553f:dfdc/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1
          接收数据包:627535 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:468397 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1000 
          接收字节:402035116 (402.0 MB)  发送字节:52318029 (52.3 MB)
```
Oh my ladygaga!!

我终于找到原因，是我的vpn路由设置不起作用导致的连不到，而最终原因却是docker建立的虚拟网桥和我们服务器的网段居然一样！！！

## 解决
找到dockerd的启动命令`/lib/systemd/system/docker.service`

修改`ExecStart=/usr/bin/dockerd -H fd:// $DOCKER_OPTS`为`ExecStart=/usr/bin/dockerd -H fd:// --bip 172.16.0.1/24 $DOCKER_OPTS`

重启docker后，在拨vpn，问题解决。
