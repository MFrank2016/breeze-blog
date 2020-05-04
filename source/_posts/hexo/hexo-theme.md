---
title: 【Hexo】使用Hexo+github pages+travis ci搭建好看的个人博客（三）
tags:
  - 环境搭建
  - Hexo
categories:
  - 编程
toc: true
date: 2020-05-04 10:40:14
---

## 说明

前两篇文章介绍了 `Hexo` + `github pages` + `travis ci` 进行自动化部署，并介绍了 `Hexo` 的配置文件中的各个属性，相信通过前两篇文章的学习，你已经学会了如何搭建自己的博客，并能够根据自己的需要进行个性化配置。

这一篇将以 `Matery` 这款主题为例，说明一下主题应该如何配置。包括主题配置、插件设置、注意事项等。

## 设置博客主题

先到[这里](https://hexo.io/themes/) 选择你喜欢的主题，

再次打开我们的配置文件(_config.yml)，修改 *theme* 属性，比如

