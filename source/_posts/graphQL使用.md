---
title: graphQL使用
categories:
- graphQL
tags:
- nodeJS
---

## graphQL 是什么
- 官方说明
> GraphQL 既是一种用于API的查询语言也是一个满足你数据查询的运行时。GraphQL对你的API中
> 数据提供了一套易于理解的完整描述，使得客户端能够准确的获得它需要的数据，而没有任何冗余，
> 也让API更容易的随时间推移而演进，还能用于构建强大的开发者工具。

- 使用 GraphQL 的好处
  - 请求的资源不多不少
  - 获取多个数据只需要发送一个请求
  - 自定义接口数据格式
  - 强大的开发工具
  - API演进无需划分版本

## 实现一个 GraphQL demo的过程
- 工具
  - 前端：JQuery 
  - 后端：nodeJS + koa + Apollo GraphQL
  > Apollo是一个开源的GraphQL开发平台。提供符合GraphQL规范的服务端和客户端实现。使用Apollo的好处在于方便进行GraphQL开发。
- 安装环境
> [https://www.mongodb.com/download-center?jmp=tutorials#community](https://www.mongodb.com/download-center?jmp=tutorials#community)
> 软件安装根目录data下新建db目录
- 或者可安装图形化工具mongoDB compass 便于数据管理
> [https://www.mongodb.com/download-center/compass](https://www.mongodb.com/download-center?jmp=tutorials#community)
- 安装 nodeJS
> [http://nodejs.cn/download/](http://nodejs.cn/download/)

- nodeJS 依赖列表
```json
{
  "apollo-server-koa": "^2.9.12",
  "koa": "^2.11.0",
  "koa-bodyparser": "^4.2.1",
  "koa-router": "^7.4.0",
  "koa-static": "^5.0.0",
  "mongoose": "^5.13.14"
}
```

- 图形化界面启动MongoDB或者bin文件夹内打开命令行
```shell
mongod --dbpath=..\data\db
```

## GraphQL API
- 服务端 apollo 添加 scheme 
> 前端发送请求需要的参数只能比预先定义的scheme少，增加未定义的参数会提示错误信息

- 启动 nodeJS 后前端请求格式
> 请求一般是get请求，请求内容为字符串类型属性
```js
$.ajax({
  url: '/graphql',
  data: {
    query: `query {
      getData{
        title
        page
        author
        desc
      }
      getData2{
        name
        sex
        age
        _id
      }
    }`
  },
  success:function (res){
    
  }
})
```

- 整体流程为：
  - 启动nodeJS 
  - 服务端定义相应的scheme
  - 链接/创建数据库
  - apollo启动中间件服务，监听GraphQL请求
  - 页面依据API以GET请求发发送graphyQL格式参数要求
  - apollo监听到GraphQL请求
  - 依据服务端apollo定义的相应Scheme返回数据。


