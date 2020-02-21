---
title: 使用阿里云OSS作为个人图床
date: 2020-02-21 20:37:37
tags:
    - 个人工具
    - 笔记
    - 图床
    - 阿里OSS
    - PicGo
    - 采坑
categories: 工具
comment : true
---


# 使用阿里云OSS作为个人图床

使用Markdown写博客的时候，有时候会需要插入图片，markdown一般都是插入的本地图片，所以当要分享到网络上，或者上传的github上的时候会失效，解决方法一般是先将本地图片上传到图床上面。

如果是到网页上传，再复制得到图片地址，过程会比较繁琐，好在已经有大神开发出了专门的可视化工具来简化操作——[PicGo](https://molunerfinn.com/PicGo/)，非常好用。本文就是使用阿里云OSS配合PicGo实现图片秒传的体验。

## PicGo界面
![picgo界面](https://cdn.jsdelivr.net/gh/Molunerfinn/test/picgo-site/first.png)
