---
title: .babelrc 详解
categories:
- babel
- JS
tags:
- babel
---

# .babelrc 详解

## 一、babel 是什么
- babel是一种js语法编译器，在前端开发过程中，由于浏览器的版本和兼容性问题，很多js的新方法和特性的使用都受到了限制。使用babel可以将代码中js代码编译成兼容绝大多数主流浏览器的代码。

## 二、安装
- 核心依赖包
  1. @babel/core：babel转译器本身，提供了babel的转译API，如babel.transform等，用于对代码进行转译。webpack的babel-loader就是调用这些API来完成转译过程的。
  2. @babel/parse：JS解释器模块内容。提供parse接口解释，最新的 ECMASCRIPT 标准，以及JSX，Flow，TypeScript，和其他实验性语言。这个模块便是以前的babylon。
  3. @babel/traverse：JS遍历AST节点模块。用于遍历解释器模块解析出来的AST节点。
  4. @babel/generator：JS生成器，主要用于将解释器解释得到的AST生成成为可解析的JS代码

- 运行过程
> input string -> @babel/parser parser -> AST -> transformer[s] -> AST -> @babel/generator -> output string

- 其他依赖包
  1. @babel/cli：命令行工具，cmd运行babel解析过程。并可以设置包括输出路径等等信息。
  2. @babel/types：用于验证，构建，和修改AST节点。
  3. @babel/polyfill：包装了core-js和 regenerate-runtime。
  4. @babel/runtime：和polyfill相似，但是不会修饰全局环境将会和plugin-transform-runtime一同使用。
  5. @babel/registor：通过使用node.js的require字段来引入。会自动的通过babel解析当前文件。

## 二、参数设置
- presets
> 预设(presets)的作用是为babel安装指定的插件，插件命名采用babel-preset-xxx的形式，安装时使用npm安装，如npm install babel-preset-es2015:
```json
{
  "presets": ["@babel/presets-react", "@babel/preset-typescript"]
}
```
- plugins
> presets是插件plugins的预设，也就是说直接需要不需要的插件一起引入，如果不想使用presets，可以单独使用plugins对某个功能进行单独的引入。另外，有一些方法是presets中不提供的，如果要使用就需要单独引用了，比如：
> plugins/presets排序遵循下列标准：
- presets 和 plugins
  1. plugins会先于presets进行编译
  2. plugins会按照数组index的增序进行编译
  3. presets会按照数组index的降序进行编译.