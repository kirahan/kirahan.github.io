---
title: 配置host文件解决GitHub上图片无法加载的问题
date: 2020-02-22 11:49:09
tags:
    - github
    - 采坑
categories: 采坑
comment : true
---

# 配置host文件解决GitHub上图片无法加载的问题
最近在登录github网站的时候，发现很多仓库的图片没法加载或者加载速度很慢，加上梯子也不太稳定，所以配置了一下github的hosts，可以加快域名解析
[类似情况](https://www.jianshu.com/p/25e5e07b2464)

1. Mac终端输入  
`sudo vi /etc/hosts`
2. 输入密码后，点击 i键，进入Insert模式，将下面内容拷贝进去
```
# GitHub Start
192.30.253.112    github.com
192.30.253.119    gist.github.com
199.232.28.133    assets-cdn.github.com
199.232.28.133    raw.githubusercontent.com
199.232.28.133    gist.githubusercontent.com
199.232.28.133    cloud.githubusercontent.com
199.232.28.133    camo.githubusercontent.com
199.232.28.133    avatars0.githubusercontent.com
199.232.28.133    avatars1.githubusercontent.com
199.232.28.133    avatars2.githubusercontent.com
199.232.28.133    avatars3.githubusercontent.com
199.232.28.133    avatars4.githubusercontent.com
199.232.28.133    avatars5.githubusercontent.com
199.232.28.133    avatars6.githubusercontent.com
199.232.28.133    avatars7.githubusercontent.com
199.232.28.133    avatars8.githubusercontent.com
 # GitHub End
```
3. `wq` 保存退出即可
