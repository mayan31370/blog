---
title: spring-boot-data-jpa的JpaRepository的分页器有点奇葩
date: 2017-09-13 11:51:40
tags: [spring,jpa,repository]
---
在使用JpaRepository时，有时我们会用到分页器，既`Pageable`。

它有个比较常用的实现`PageRequest`，但它的构造方法中`page`比较奇葩，是从0开始的，这和我们常见的前端框架是不一致的，特此记录。
