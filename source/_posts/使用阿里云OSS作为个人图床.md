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

使用**Markdown**写博客的时候，有时候会需要插入图片，markdown一般都是插入的本地图片，所以当要分享到网络上，或者上传的github上的时候会失效，解决方法一般是先将本地图片上传到图床上面。

果是到网页上传，再复制得到图片地址，过程会比较繁琐，好在已经有大神开发出了专门的可视化工具来简化操作——[PicGo](https://molunerfinn.com/PicGo/)，非常好用。本文就是使用阿里云OSS配合PicGo实现图片秒传的体验。

## PicGo界面
![picgo界面](https://cdn.jsdelivr.net/gh/Molunerfinn/test/picgo-site/first.png)

## PicGo支持的图床（还可以通过插件自定义图床）
- SM.MS
- [腾讯云](https://cloud.tencent.com/)
- 微博
- [Github](https://www.jianshu.com/p/2756724a5dee)
- [七牛](https://www.qiniu.com/)
- [Imgur](http://www.imgur.com/)
- [阿里云](https://github.com/PicGo/Awesome-PicGo)
- [又拍云](https://www.upyun.com/)

>PicGo的相关插件合集 [Awesome-PicGo](https://github.com/PicGo/Awesome-PicGo)

---
## 使用阿里云OSS作为图床
先来看看PicGo中阿里云OSS的配置项
![](http://imagebed.solarsunrise.cn/blog/img/20200221233839.png)  
通过下面四步我们就可以获得上面这些配置信息
1. 注册和购买阿里云OSS服务
2. 新建OSS中的bucket
3. 获取KeyID和KeySecret
4. 配置私有域名

### 注册和购买阿里云OSS服务
#### 1. 购买阿里OSS  
   OSS服务其实很便宜，一年也才几块钱，建议开发者都可以买一个
![](http://imagebed.solarsunrise.cn/blog/img/20200221220754.png)
#### 2. 新建OSS中的bucket  
   bucket相当于是一个一个独立的容器，所以一般一个应用设置一个bucket，比如这里专门建立一个bucket作为图床

**新建bucket**  
bucket名字自己设置，类型选择标准，一定不能错的是读写权限：***公共读***
即只有你可以上传文件，其他人仅可以访问和读取
![](http://imagebed.solarsunrise.cn/blog/img/20200221233217.png)

#### 3. 获取KeyID和KeySecret
   通过key和secret来做身份认证，也就是要填入PicGo中的关键参数，这里需要强调的是，从风险规避的角度，务必在阿里云中应该申请子用户AccessKey，通过子用户赋予这个keyid仅仅操作OSS的权限而不是完整权限的key  
   ![](http://imagebed.solarsunrise.cn/blog/img/20200221234114.png)

---

![](http://imagebed.solarsunrise.cn/blog/img/20200221233839.png)  
> * 第一项、第二项为第3步生成的keyid和keysecret  
> * 第三项就是你创建的bucket名称  
> * 第四项是OSS的存储区域，类似于使用的那个节点，结构一般都是*oss-cn-城市* 在bucket页面中可以找到

前四项填好之后OSS图床其实已经可以工作了，我们上传一张图片：
图片拖动到顶部PicGo图标，等待片刻图片地址以及markdown就会自动放入剪切板，粘贴使用即可  
![](http://imagebed.solarsunrise.cn/blog/img/20200221235708.png)

剪贴板中信息大概如下：
```
![](http://kirahan-blog-photobed.oss-cn-beijing.aliyuncs.com/blog/img/20200221235708.png)
```

# 坑：
此时通过图片URL访问，会发现图片无法预览，在浏览器打开图片url会自动下载  
![](http://imagebed.solarsunrise.cn/blog/img/20200222000606.png)  
出现这种问题不是设置的问题，而是阿里云做的限制[说明文件](https://help.aliyun.com/document_detail/142631.html)，解决方案是配置私有域名
> 对于图片文件（在未修改文件http头的情况下）：
若您的Bucket是2019年9月23日前创建的，使用OSS默认访问域名或自有域名生成的文件URL从浏览器访问时可以预览文件内容。
若您的Bucket是2019年9月23日后创建的，使用OSS默认域名生成的文件URL从浏览器访问时会以附件形式下载；使用自有域名生成的文件URL访问时，可以预览文件内容。绑定自有域名步骤请参见绑定自定义域名。
https://help.aliyun.com/document_detail/142631.html
#### 4. [配置私有域名](https://help.aliyun.com/document_detail/31902.html?spm=a2c4g.11186623.2.11.55207119cNLRuT#concept-ozw-m2r-5fb)
绑定自定义域名的前提条件
- [x] 拥有个人域名
- [ ] 域名已经备案

配置步骤：
1. 进入目标Bucket，打开域名管理页签
2. 单击传输管理 > 域名管理
3. 在绑定用户域名对话框配置以下参数
![](http://imagebed.solarsunrise.cn/blog/img/20200222001822.png)
4. 在域名解析控制台添加对应的解析记录
![](http://imagebed.solarsunrise.cn/blog/img/20200222002012.png)
5. 不要忘了在PicGo中填上你的私人域名，这样剪切板中的链接就会变成域名开头的
![](http://imagebed.solarsunrise.cn/blog/img/20200221233839.png) 

---
最后大功告成！可以愉快的写博客了
![](http://imagebed.solarsunrise.cn/blog/img/20200222002455.png)