---
title: WCA官方API详解
date: 2020-02-29 12:04:56
tags:
    - WCA
    - API接口
    - 魔方
    - 文档
categories: WCA
---
# WCA官方API详解
**WCA是有提供API接口的，但没能在[WCA官网](https://www.worldcubeassociation.org/)上找到具体的接口文档，也没有发现有谁已经整理过了，所以这里做一个简单的梳理**

## 0. 授权登录
wca的接口都是需要授权之后才可以发送请求的，使用的是标准的Oauth2.0，稍后会写一篇文档记录如何做授权登录 

## 1. 参考链接
WCA官网是用哪个ruby写的，完全看不懂，所以检索了一下[WCA的源代码](https://github.com/thewca/worldcubeassociation.org)，找到几个关键文件：

* [全部路由表](https://github.com/thewca/worldcubeassociation.org/blob/master/WcaOnRails/config/routes.rb) 这里定义了所有路由，从中可以找到所有的api接口，不过没有带参数说明
* [API部分的controller](https://github.com/thewca/worldcubeassociation.org/blob/2f2a1b591a64fcef5cfb5ad7e0ee01a1e61f4e21/WcaOnRails/app/controllers/api/v0) 包括api部分以及person，地理位置geocoding和competition
* [全部controller](https://github.com/thewca/worldcubeassociation.org/tree/2f2a1b591a64fcef5cfb5ad7e0ee01a1e61f4e21/WcaOnRails/app/controllers) 部分接口不在api/v0中的，可以从全部的controller中找到
* [API的spec文件](https://github.com/thewca/worldcubeassociation.org/blob/7f13c6f67c8187c0f0b5c94cbac633d2c2564b01/WcaOnRails/spec/controllers/api_controller_spec.rb)ruby的spec好像类似于node中的test文件，属于测试文件，其中有一些用例，也有一些参数说明，这个文件帮助很大，避免了进一步去看源码了
* [比赛相关的spec文件](https://github.com/thewca/worldcubeassociation.org/blob/7f13c6f67c8187c0f0b5c94cbac633d2c2564b01/WcaOnRails/spec/controllers/api_competitions_controller_spec.rb) 包含了赛事相关的接口用例
* [Person相关的sepc文件](https://github.com/thewca/worldcubeassociation.org/blob/7f13c6f67c8187c0f0b5c94cbac633d2c2564b01/WcaOnRails/spec/controllers/persons_controller_spec.rb) 包含了person模块的接口用例

## 2. 主要接口
- `/me`  返回授权登录的用户信息【不需要传参】  
返回值如下
![](http://imagebed.solarsunrise.cn/blog/img/20200229131601.png)
- `export_public` 下载数据库
- `scramble-program` 获取打乱程序信息  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229132354.png)
- `search` 模糊查询接口,可以查询 competition、user、regulation  
  返回的可以是比赛也可以是人
- `/search/posts` 查询推送接口,**[有参数][必须]** `{q : "post's title or body"}` 优先搜索标题，其次搜索正文  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229134734.png)
- `/search/competitions` 搜索比赛接口，**[有参数][必须]** `{q : "competition"}`   
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229135311.png)
  返回值中有很多有用参数：名字、id、时间、地理位置、地址、代表、主办方、项目
- `/search/users` 搜索选手接口，**[有参数][必须]** 一共有四种格式  
  `{q : "usersname" or "wcaid"}`  一般情况只需要传字段q  
  `{q : "emailaddress" , email:true}`  通过邮件地址搜索用户
  `{q : "usersname" or "wcaid" , persons_table : true or false}` 增加了persons_table，[原文描述](https://github.com/thewca/worldcubeassociation.org/blob/7f13c6f67c8187c0f0b5c94cbac633d2c2564b01/WcaOnRails/spec/controllers/api_controller_spec.rb#L108)是：
  >Person without User

    应该是那种有wcaid的用户吧  
  `{q : "usersname" or "wcaid", include_dummy_accounts: true or false}` 增加了 include_dummy_accounts字段，dummy意思是假的，可能也是一种特殊的用户类型吧
  返回值格式都一致
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229150346.png)
- `/search/regulations` 搜索比赛规则 **[有参数][必须]** `{q : "title"}` 可以检索标号和关键字  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229150945.png)
- `/users/:id` 获得用户资料   **[无参数]** id直接通过/:id的params形式传入，用于获取user资料
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229151854.png)

- `/users/:wca_id` 获取用户资料  **[无参数]** wcaid直接通过/:wca_id的params形式传入，用于获取user资料  
  返回值如下

  ![](http://imagebed.solarsunrise.cn/blog/img/20200229151854.png)
- `/delegates` 获取wca代表信息  **[无参数]**   
  返回值如下

  ![](http://imagebed.solarsunrise.cn/blog/img/20200229152906.png)
- `/persons` 获取选手的比赛详情 ***区别于users和search接口中的选手资料，proson接口获取的是比赛成绩相关数据*** **[有参数][可选]**  有几种参数可以定义
   1. 不含参数，会按照wcaid的先后返回数据
   2. `params: { q: "name"}` 包含一个search字段，可以是名字也可以是wcaid
   3. `params: { wca_ids: "wcaid1,wcaid2,wcaid3" }` 通过wcaid查询，可以是一个id也可以是多个id 用per_page=来控制个数  
   返回值如下
   ![](http://imagebed.solarsunrise.cn/blog/img/20200229160640.png)
- `/persons/:wca_id` 获取选手的比赛详情直接将wcaid填入url中 **[无参数]**  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229160930.png)
- `/persons/:wca_id/results` 按照选手参加的赛事排序返回选手成绩 **[无参数]**  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229161211.png)
- `/geocoding/search`  搜索地理位置 没看出来应该填什么参数
    ```
    query_params = {
      address: params.require(:q),
      key: ENVied.GOOGLE_MAPS_API_KEY,
    }
    render json: JSON.parse(RestClient.get(GMAPS_GEOCODING_URL, params: query_params).body)
    ```
- `/records` 获取所有记录，包括世界、大洲、国家 **[无参数]**  但是只有成绩，没有选手信息  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229162251.png)
## 3.赛事管理相关接口
api中有很多管理赛事的子接口，所以单独列出
- `/competitions/:competition_id` 获取比赛信息，直接将competition_id如：WuhanAutumn2019 填入url中 **[无参数]**    
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229164410.png)

- `/competitions/:competition_id/results` 获取比赛成绩 **[无参数]**   
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229165314.png)
- `/competitions/:competition_id/competitors`获取比赛选手名单 **[无参数]**   
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229165238.png)
- `/competitions/:competition_id/registrations`获取比赛报名名单 **[无参数]**   
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229165522.png)
- `/competitions/:competition_id/schedule`获取比赛赛程明细 **[无参数]**   
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229165157.png)
- `/competitions/:competition_id/wcif`获取wcif信息，需要赛事管理员权限，后续补充
  
- `/competitions/:competition_id/wcif/public`获取wcif公开信息 **[无参数]**  
  返回值如下
  ![](http://imagebed.solarsunrise.cn/blog/img/20200229165737.png)


