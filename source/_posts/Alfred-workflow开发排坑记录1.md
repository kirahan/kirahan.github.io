---
title: Alfred-workflow开发排坑记录1
date: 2020-02-29 00:31:08
tags:
    - 个人工具
    - MAC
    - alfred
    - 采坑
categories: 采坑
---

# 采坑记录
## 一句话总结，建议直接看官方文档，网上的教程都不清晰，也不是最新的，很多都失效了，官方文档的说明还是很全面的。  
![](http://imagebed.solarsunrise.cn/blog/img/20200229004634.png)
[官方说明](https://www.alfredapp.com/help/workflows/)
1. 不要用python来开发，python只支持python2，不支持python3
2. 搜索列表呈现主要选用插件     `ScriptFilter`
3. 使用nodejs，读取环境变量为`process.env`,但是只能读不能写
4. 另外只有通过workflow启动的js脚本能读取出process.env
5. 读取输入参数用 `process.argv[xx]`
6. 呈现ScriptFilter使用console.log() json数据接口即可，不能增加其他的console.log() 否则会出错    `反人类`
7. ScriptFilter中icon配置项，文档中的icon.type = 'filetype' 是指填入具体的图片格式如 `png，jpg`等，而不是直接写filetype， `巨坑`
8. ScriptFilter数据结构中有很多有用的参数，如自动全autocomplete，valid，甚至可以给每个item设置变量，非常强大
9. ScriptFilter不能用定时函数，直接用json文件中的`rerun`参数即可
10. 插件中还有一个JSON Utility的模块，非常强大，还没来得及研究
