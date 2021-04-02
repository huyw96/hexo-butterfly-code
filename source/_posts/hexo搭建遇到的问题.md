---
title: hexo搭建遇到的问题
categories: hexo
top_img: >-
  https://cdn.jsdelivr.net/gh/huyw96/hexo-butterfly-code@v1.0/source/img/bg/bg3.png
cover: >-
  https://cdn.jsdelivr.net/gh/huyw96/hexo-butterfly-code@v1.0/source/img/article/hexo.png
tags:
  - hexo
  - 问题
abbrlink: 8e9a1d29
date: 2021-01-01 12:00:00
---
### 更换主题后报错  
> extends includes/layout.pug block content include includes/recent-posts.pug include
### 解决方法:
```
npm install --save hexo-renderer-jade hexo-generator-feed hexo-generator-sitemap hexo-browsersync hexo-generator-archive
```