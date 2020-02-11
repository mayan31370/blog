---
title: 用hexo和github搭建自己的静态博客
date: 2017-03-08 08:15:08
tags: 
  - git
  - hexo
categories: 
  - 工具
---
# 准备工作
## 创建github仓库
首先，你要有一个github帐号，什么？你还没有github帐号？那你已经错过了很多非常棒的开源软件了~！[快去注册吧](http://github.com)

然后，你需要创建一个仓库名为 {你的username}.github.io 的仓库
## 安装git
没有git？[快去下载吧](http://git-scm.org)
## 安装hexo
这里就不搬运了，去官网看[教程](https://hexo.io)吧。
# 日常使用
- 创建新博文`hexo new post <title>`
- 生成静态文件`hexo g`
- 本地模拟`hexo s`
- 部署到远端`hexo d`

# 撰写博文的一些小技巧
## tag的使用
支持多个tag，语法：`tags: [git,hexo]`
## markdown常用语法
### 表格
```markdown
| Header One     | Header Two     |
| :------------- | :------------- |
| Item One       | Item Two       |
```
### 斜体
```markdown
*内容*
```
### 粗体
```markdown
**内容**
```
### 标题
```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
```
### 链接
```markdown
[文本](链接)
```
### 其他特性可以用html标签
