---
title: 怎么使用hexo搭建博客
date: 2022-02-23 11:41:10
tags:
- 博客           //多个标签可以这样添加
- hexo
categories: 博客
---
###在博文中添加图片
(1)在全局配置文件（hexo/_config.yml)中将post_asset_folder设置为true；
(2)创建文章（在创建的时候，会在hexo/source/_post目录下，生成一个XXX.md文件和一个XXX的文件夹）
hexo new "XXX"
(3)把XXX这个博文需要展示的图片放在XXX文件夹目录下；
(4)在XXX.md文件中引入图片的方式：

{% asset_img example.jpg This is an example image %}

{% asset_img example.png This is an example image %}

