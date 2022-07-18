---
title: Vue SSR方案
categories:
- SSR
- Vue
tags:
- SSR
- nuxt
---

## 方案
> 服务端渲染方案一般是用于对首屏要求较高的场景，或者SEO较好的优化效果，移动端也经常使用。
> 当前有官方推荐方案、Vite方案、nuxt2.x方案

### Vue3官方方案
> 使用express匹配路由进行渲染操作，这种简单的组合适用于不复杂的静态资源页面
```js
import express from 'express';
import { renderToString } from 'vue/server-renderer';
import { createSSRApp } from 'vue';

const server = express();

server.get('/', (req, res) => {
  const app = createSSRApp({
      data: () => ({ count: 1 }),
      components: {
          
      },
      template: `<div @click="count++">{{ count }}<Home/></div>`,
      // router: router,
  });;

  renderToString(app).then((html) => {
    res.send(`
    <!DOCTYPE html>
    <html>
      <head>
        <title>Vue SSR Example</title>
        <script type="importmap">
          {
            "imports": {
              "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
            }
          }
        </script>
        <script type="module" src="/client.js"></script>
      </head>
      <body>
        <div id="app">${html}</div>
      </body>
    </html>
    `);
  });
});

server.use(express.static('.'));

server.listen(3000, () => {
  console.log('ready');
});

```

### Vite + koa + vue-router实现SSR
#### 开发环境使用创建ViteServer.middleware服务链接koa-connect

#### 生产环境
- client.js用于客户端激活路由文件 ->
- service.js文件激活服务端路由监听及第一次服务端渲染字符串 ->
- 生产环境客户端客户端文件夹构建 /dist/client 生成路由映射关系mainfest.json ->
- 生产环境服务端文件夹构建 /dist/server ->
- 启动服务端监听 ->
- 路由接收第一个页面请求 ->
- koa路由监听到路由进入处理页面 ->
- 区分 /assets 客户端打包文件夹或者服务端第一次渲染处理 ->
- 第一次服务端请求使用Vite renderString函数结合path服务端渲染于HTML的字符串 ->
- HTML字符串返回至客户端页面直接渲染，并启动客户端路由 ->
- 已激活的客户端路由请求静态资源文件均为 /assets 路径下，后续请求不再进行服务端渲染

### nuxt方案

#### nuxt2.x
> 2.x版本模板较全，可选nodeJS框架及SSR模板等内容
- 安装
```shell
npm install create-nuxt-app -g

npx create-nuxt-app init app-name 
```
- 选项
```shell
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


#### nuxt3.x
> 基于Vue3.x,安装即支持SSR，增加pages等文件夹及配置即可使用
> 官网(https://v3.nuxtjs.org/getting-started/migration)
- 安装
```shell
npm install nuxi -g

npx nuxi init app-name
```



