---
title: gitflow应用
date: 2017-03-07 20:29:28
tags: [git,gitflow]
---
git版本控制提供灵活的分支管理工具。在我们的实际生产应用中怎样使用分支来做好自己的版本控制?gitflow提供了一整套解决方案。
## 什么是gitflow？
- 它是git的分支管理思想
- 它也是git的分支管理工具

## 思想
首先，在gitflow中，将分支做了一个分类：
1. master 主分支 一般用于最终发布的分支
2. hotfix 紧急修正分支 一般用于快速修复生产环境的阻碍性bug的修复补丁
3. dev 开发分支 在开发中的核心分支
4. feature 特性分支 用于完成特定需求而建立的分支
5. release 发布分支 用于特定版本的构建

其次，每种分支都有其独特的工作流：

| 分支类型 | 创建 | 完成 | 备注 |
| :---: | :---: | :---: | :---: |
| master | - | - | 发布分支，默认创建 |
| dev | - | - | 开发分支，默认创建 |
| hotfix | 从master上新建分支 | 将hotfix分支合并到master分支<br>在master分支上打tag<br>删除hotfix分支 | |
| release | 从dev上新建分支 | 将release分支合并到dev分支<br>将release分支合并到master分支<br>在master分支上打tag<br>删除release分支 | |
| feature | 从dev上新建分支 | 将feature分支合并到dev分支<br>删除feature分支 | |

也就是说，gitflow的每个操作都是固化的一组git命令

## 命令

### 初始化

```shell
git flow init
```

接下来会提几个问题：
1. 主分支名 master
2. 开发分支名 dev
3. 各种分支和tag前缀名 直接回车 使用默认的即可

### 获取使用帮助

```shell
git flow
```

将列出可用的命令及其功能

```shell
git flow XXX help
```

XXX为`git flow`中列出的命令

将列出具体命令支持的操作

### 常见操作

| 操作名 | 支持分支 | 功能 |
| :---: | :---: | :---: |
| start | feature,release,hotfix | 创建分支 |
| finish | feature,release,hotfix | 完成分支，自动出发完成分支的一系列操作 |
| publish | feature,release,hotfix | 提交分支到远端 |
| delete | feature,release,hotfix | 删除分支 |
| list | feature,release,hotfix | 罗列该分类下所有分支 |
| track | feature,release | 跟踪远端分支 |
| checkout | feature | 切换远端分支 |
| pull | feature | 拉去远端分支上的内容 |

## 实践

| 分支类型 | 命名规整 | 示例 |
| :---: | :---: | :---: |
| hotfix | {release}patch{No.} | v2.2patch1 |
| release | v{version code} | v2.2 |
| feature | rm-{redmine issue id} | rm-saas-38 |

`备注：由于feature大部分情况下为单个人员完成，所以只需维护本地分支，分支名可以起自己看得懂的即可`
