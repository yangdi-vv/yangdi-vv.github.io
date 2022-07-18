---
title: ESbuild
categories:
- JS
- 构建
- 开发工具
tags:
- ESbuild
---

## 介绍
> ESbuild是一款基于GOLang的打包工具，相比于传统打包工具速度提升达到数十倍

- 优点
  - 基于GO，相比较于JS，GO是机器语言，速度更快
  - JS是单线程，ESbuild是多线程并行
  - 没有使用AST，优化了构建流程

- 缺点
  - 没有 TS 类型检查
  - 不能操作 AST
  - 不支持装饰器语法
  - 产物 target 无法降级到 ES5 及以下


![对比](http://101.34.242.163/img_blog/esbuild.jpg)
![对比](http://101.34.242.163/img_blog/esbuild.png)

## ESM及bound(webpack等)执行机制
![](http://101.34.242.163/img_blog/webpackVSvite.png)

- webpack
> Webpack 的构建流程简单来说就是递归编译每一个模块文件，对于不同类型的文件使用不同的 webpack loader 进行处理。
> 并可以自动构建并基于你所引用或导出的内容推断出依赖的图谱。
> esbuild-loader 是一个构建在 esbuild 上的 webpack loader，且可以替代 babel-loader 或 ts-loader 来提高构建速度。

- vite
> esbuild 构建默认开启了 Tree shaking（sub 并没有被编译输出），还可以进行 minify、 sourcemap，指定 platform ，以及 watch（监视模式）等等。具体可以查看官方 [API](https://esbuild.github.io/api/)

## 使用
- 安装
```shell
npm i esbuild -g
```

- 运行
```shell
$ esbuild main.ts
```

- 编译代码
``````

- 处理JSX
- 压缩

## 结语
>在当前前端环境中，直接使用 esbuild 代替 webpack 不现实；主流方案是在 webpack 中使用 esbuild 去做一些代码的 transform (代替 babel-loader)。当然随时后续 vite（采用 esbuild 预构建依赖） 、snowpack 等构建工具的发展，其会在某些场景下替代 webpack（vue3 的官方推荐构建工具为 vite）。esbuild 针对构建 应用 的重要功能仍然还在持续开发中 —— 特别是代码分割（可以获得最佳的加载性能）和 CSS 处理方面。当未来这些功能稳定后，不排除使用 esbuild 作为生产构建器的可能。

>vite 中引用 esbuild： https://github.com/vitejs/vite/blob/main/packages/vite/src/node/optimizer/index.ts#L5

>webpack 在 v5 版本中也是针对编译的性能做出了不少努力，除了提供了物理缓存的优化之外，还提供 Module Federation 的方案，这给我们上层的应用实践带来了很多想象的空间。以前 webpack 大有一统构建工具的趋势，而现在我们可以结合业务的特点有更多的选择。
