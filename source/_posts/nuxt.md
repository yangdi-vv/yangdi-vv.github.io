---
title:
categories:
- Vue
- Nuxt
- JS
tags:
- Nuxt
---

## Nuxt
> 官网：https://v3.nuxtjs.org/getting-started/quick-start

## SSR
> 一般我们开发的应用多属于SPA，在客户端进行渲染（CSR），而在服务端进行渲染后直接将渲染完成的HTML以
> 字符传回客户端处理的形式称为SSR
![](http://101.34.242.163/img_blog/nuxt1.png)

- 优点
  - SEO
  - 内容到达时间（time-to-content）时间更短
  
- 缺点
    - 开发环境需要有nodeJS 支持
    - 代码开发需由客户端开发以外的部分插件等支持
    - 对服务器性能要求较高

## Nuxt 介绍
> Nuxt是基于Vue和服务端的渲染应用框架，主要应用于SSR（服务端渲染），对SEO较友好
> 如果不是SSR或者SEO要求较高的项目建议使用vue-cli/vite/webpack等可能更加便于开发，
> 毕竟Nuxt主要是进行了SSR的封装，非SSR项目使用NUXT没有什么特别的优点

- 渲染方式
> NUXT build

- NUXT 流程图
  ![](http://101.34.242.163/img_blog/nuxt2.png)

- NUXT 编译过程
  ![](http://101.34.242.163/img_blog/nuxt3.png)

- 特性
```text
基于 Vue.js
Vue、Vue Router、Vuex、Vue SSR
自动代码分层
服务端渲染
强大的路由功能，支持异步数据
静态文件服务
ES2015+ 语法支持
打包和压缩 JS 和 CSS
HTML 头部标签管理
本地开发支持热加载
集成 ESLint
支持各种样式预处理器： SASS、LESS、 Stylus 等等
支持 HTTP/2 推送
```


### 安装及使用
- 2.x
```shell
npm install create-nuxt-app -g

npx create-nuxt-app appName
```
- 安装过程中会有提示选择需要的插件
```text
yd@bogon nuxt-ssr % npx create-nuxt-app nuxt-ssr                           

create-nuxt-app v4.0.0
✨  Generating Nuxt.js project in nuxt-ssr
? Project name: nuxt-ssr
? Programming language: TypeScript
? Package manager: Npm
? UI framework: (Use arrow keys)
? UI framework: 
? UI framework: Ant Design Vue
? Nuxt.js modules: Axios - Promise based HTTP client, Progressive Web App (PWA)
? Linting tools: ESLint, Prettier, Lint staged files, StyleLint, Commitlint
? Testing framework: None
? Rendering mode: Universal (SSR / SSG)
? Deployment target: Server (Node.js hosting)
? Development tools: (Press <space> to select, <a> to toggle all, <i> to invert selection)
? Continuous integration: None
? Version control system: Git
⠋ Installing packages with npm
```

- 3.x
```shell
npm install nuxi -g

npx nuxi init appName
```

> 使用方式和2.x一样，不过创建后省略了很多的文件夹和配置，整体显得清爽些，
> 初始化后直接可以创建/pages/xxx.xue，并生成相应路由和文件

### 部署方式

#### 构建
```shell
nuxt build //动态页面 服务端实时渲染后返回

nuxt generate //静态页面 包含请求数据
```
