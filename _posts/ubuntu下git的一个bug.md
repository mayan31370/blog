---
title: Ubuntu下git的一个bug 
date: 2018-02-01 14:02:57
tags: [ubuntu,git]
---
在使用ubuntu的时候，偶然会遇到在clone项目时出现错误`error: RPC failed; curl 56 GnuTLS recv error (-110): The TLS connection was non-properly terminated.`
这是一个从12.04版本开始出现的bug，并且一直没有解决。
但是这个问题并不会总是出现，它和项目的大小有着一定的关系。

# 一个快速的解决方案
虽然我们不能clone，但是可以pull/push。所以可以绕开clone命令。
```shell
mkdir {项目文件夹}
cd {项目文件夹}
git init
git remote add origin {项目地址}
git pull
```
这是最简单的一个方案，但是如果你要使用其他gui的客户端，就会遇到问题。

# 一个不完善的解决方案
通过设置git的参数，可以在较小的项目中避开报错。
```shell
git config http.sslVerify false
git config --global http.postBuffer 1048576000
```
这个方案比较简单，单后遗症最大，碰到稍大一点的项目就会报错，所以最不建议此方案。

# 完美解决方案
这个报错其实是libcurl-gnutls模块的一个bug，想避免此bug，可以重新编译git并让其使用libcurl-openssl来替代libcurl-gnutls。
这个方案能完美的解决问题，但比较麻烦，编译的步骤如下：
  1. 安装依赖工具
```shell
sudo apt-get install build-essential fakeroot dpkg-dev
```
  1. 创建编译git的文件夹并进入
```shell
mkdir ~/git-rectify
cd git-rectify
```
  1. 下载git源码
```shell
apt-get source git
```
  1. 安装git依赖
```shell
sudo apt-get build-dep git
```
  1. 安装openssl模块
```shell
sudo apt-get install libcurl4-openssl-dev
```
  1. 解压源码包
```shell
dpkg-source -x git_xxxx.dsp
```
  1. 修改编译配置 git_xxx/debian/control 中 libcurl4-gnutls-dev 替换为 libcurl4-openssl-dev
  1. 修改编译配置 git_xxx/debian/rules 中 删除 TEST=test 行
  1. 打包（在git_xxx目录下）
```shell
sudo dpkg-buildpackage -rfakeroot -b
```
  1. 安装编译好的git
```shell
sudo dpkg -i git_xxx_amd64.deb
```
