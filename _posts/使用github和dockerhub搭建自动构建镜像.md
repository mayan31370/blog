---
title: 使用github和dockerhub搭建自动构建镜像
date: 2017-11-07 10:13:28
tags: [git,github,docker]
---
最近，在工作中需要构建一些docker的公共基础镜像，但每次进行微调时都比较麻烦，所以想自动构建，正好github和dockerhub提供了现成的解决方案。

# 准备阶段
- github帐号
- docker帐号

# 步骤
1. 创建github仓库
1. 在docker中的setting里选择Linked Accounts & Services，绑定github帐号
1. 在docker中，要点击create下拉按钮，选择Create Automated Build
1. 选择好仓库即可
