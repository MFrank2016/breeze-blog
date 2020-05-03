---
title: hexo-theme-matery 主题音乐插件的配置
tags:
  - 环境搭建
  - Hexo
  - hexo-theme-matery
originContent: ''
categories:
  - 编程
toc: true
---


音乐插件使用的是 Aplayer 播放器，可以选择网易云音乐，然后设置自己的榜单id，但我发现榜单更新后插件里但列表不会更新，所以好奇之下，想看看它的数据是从哪里来的。

最终在 https://cdn.jsdelivr.net/npm/meting@2/dist/Meting.min.js 这个文件里找到里答案，数据是从这个接口获取的：`https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r`，猜测是直接将获取到的榜单列表写进里数据库，但后续不会进行更新，所以导致无论多少次刷新，都只能获取第一次取到的数据。

有两种解决办法，第一种是创建新歌单，然后一次性添加足够多的歌，然后在配置文件中替换成你的歌单id，另一种是自己写一个网易云音乐歌单解析接口，来获取网易云音乐数据。