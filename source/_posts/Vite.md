---
title: Vite
categories:
- Vue
- 部署
tags:
- Vite
- ESbuild
- Rollup
---

## Vite 介绍
> Vite 基于原生 ES module 的一个开发服务器，相比较webpack的启动过程，Vite可以直接开启开发服务器，浏览器请求时再
> 返回浏览器支持的ES模块，不需要全部打包依赖的文件后再启动，大大减少了开发启动时间。

- 安装
```shell
npm install vite@latest
```

- 初始化
```shell
npm init vite project-name
```

- 使用
```js
import { defineConfig } from 'vite';

export default {
    
}
#或者
export default defineConfig(({commond, mode}) => {
    return {}
})
```

- Vite核心模块
  > - ESbuild: 进行依赖预构建，也是开发环境速度快的主要原因
  > - Rollup: 用于项目的打包，因此Vite的插件也是受限于Rollup的插件

## ESbuild

- 内置支持 
  > - ESbuild默认会编译ts tsx jsx file，默认不编译js文件
  > - HMR


- 依赖预构建 
  > - commonJS 和 UMD 等转义为ES模块，这些模块浏览器天然支持
  > - 打包第三方库，将多个内部模块的ES模块的依赖关系转化为单个模块，因为每个ES模块浏览器都会发送一个请求，减少了ES模块请求提高性能。


- 为什么很快
  > - js是单线程串行，esbuild是新开一个进程，并且多线程运行，发挥多核优势，同时esbuild使用GO编写，GO是机器语言比JIT（即时编译）快
  > - 不使用AST优化编译流程
![对比图](http://101.34.242.163/img_blog/esbuild.jpg)


- 缓存
  > - Vite会将与构建的依赖缓存存放至.vite文件夹，切依据package.json及.lock文件来界定是否重新运行构建步骤。
  > - 解析后的依赖请求会以HTTP头 max-age=31536000,immutable 设置强缓存，一旦设置强缓存后这些依赖将不会再到达开发服务器，重启Vite服务或者浏览器禁用缓存可使强缓存失效

- 兼容
  > - esbuild也有不支持的语法，比如装饰器@等，需要安装其他插件来支持
  > - Vite 对浏览器的要求较新的版本， 提供了一个插件 @vitejs/plugin-legacy 用来将代码打包出一个兼容性较好的版本

## Vite插件和Rollup
> - ESbuild是一个在构建库方面比较出色的工具，但是在生产环境中对代码分割、和CSS的处理方面还在完善中，因此Vite在成产环境中使用了比较完善成熟Rollup.
> - Vite的构建打包依赖Rollup, 因此Vite的插件限制于Rollup插件。



## 对比Webpack
- webpack服务器启动过程依赖入口及路由等依赖一步步查找收集，编译成一个或者多个JS后再启动服务器，越复杂的项目启动时间影响越大，但社区活跃且插件种类丰富。


- Vite直接开启服务器，预构建打包第三方依赖的模块，由于基于浏览器支持的ES module，浏览器请求文件时再返回浏览器支持的文件，实现了懒加载文件，开发环境速度提升较明显，


- 但由于出现时间较短，插件等第三方支持还不完善。


- 官方对比图
![对比图](http://101.34.242.163/img_blog/webpackVSvite.png)

## Vite优缺点
- 优点：
  > 一个字 —> 快
- 缺点
  > 第三方插件还不完善，需要时间

## 结语
> 简单的说Vite的启示是浏览器能做的事还是给浏览器去做，短期内Vite还有待补全，但未来可期