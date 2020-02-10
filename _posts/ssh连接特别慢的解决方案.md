---
title: ssh连接特别慢的解决方案
date: 2017-08-17 14:31:27
tags: [ubuntu,ssh]
---
最近做了不少运维工作，一个之前一直存在但被忽略的问题显的越发的讨厌了。

就是，当ssh连接服务器的时候，在建立的时候非常慢，但连接上以后，就恢复正常了，今天实在是忍无可忍，终于动手解决了。

## 定位问题
1. 确认网络没有问题
1. 确认ping到服务器的链路正常
1. 使用`ssh -v host`来查看慢的原因

发现问题出现在此处：
```
debug1: Next authentication method: gssapi-with-mic
debug1: Unspecified GSS failure. Minor code may provide more information
No credentials cache found
```
连接在此处会有一个长时间的等待

经查询，是在authentication gssapi-with-mic时，消耗了大量的时间

## 解决问题
这个属于客户端的问题，比较好改。

编辑`/etc/ssh/ssh_config`,修改：`GSSAPIAuthentication yes`为`GSSAPIAuthentication no`

问题解决，并没有花费多长时间。
