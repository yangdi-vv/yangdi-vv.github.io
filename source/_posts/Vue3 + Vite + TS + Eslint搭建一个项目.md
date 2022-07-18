---
title: Vue3 + Vite + TS + Eslint搭建一个项目
categories:
- Vue
- Typescript
- Vite
- Eslint]
tags:
- 笔记
---

## 安装Vue3

```shell
npm install vue@next
or
yarn add vue@next
```

## 安装Vite
```shell
npm install vite
or
yarn add vite
```

## 初始化Vue项目
```shell
npm init vite app-name --template vue
or
yarn create vite app-name --template vue
```

## 安装Eslint

- 安装
```shell
npm install eslint -D
```

- eslint初始化命令行
```shell
npx eslint --init
```

- eslint初始化选项
```shell
You can also run this command directly using 'npm init @eslint/config'.
√ How would you like to use ESLint? · problems
√ What type of modules does your project use? · esm
√ Which framework does your project use? · vue
√ Does your project use TypeScript? · No / Yes
√ Where does your code run? · browser, node
√ What format do you want your config file to be in? · JavaScript
The config that you've selected requires the following dependencies:

eslint-plugin-vue@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
√ Would you like to install them now? · No / Yes
√ Which package manager do you want to use? · npm
Installing eslint-plugin-vue@latest, @typescript-eslint/eslint-plugin@latest, @typescript-eslint/parser@latest

up to date, audited 233 packages in 2s
```

- 修改eslintrc.js

> .eslintrc.js中删除 ~~"plugin:vue/essential"~~,否则会报错
```shell
"extends": [
        "eslint:recommended",
        "plugin:vue/essential",
        "plugin:@typescript-eslint/recommended"
    ]
    
或者修改为:

"extends": [
        "eslint:recommended",
        "plugin:vue/vue3-essential",
        "plugin:@typescript-eslint/recommended"
    ]    
```
> 添加parser: 'vue-eslint-parser',
 

- 修改vite.config.ts
```shell
import eslintPlugin from 'vite-plugin-eslint';

export default defineConfig({
  plugins: [vue(), eslintPlugin()]
})
```

- 运行
  package.json运行
```shell
"lint": "eslint src/**/*.{js,jsx,vue,ts,tsx} --fix"
```
  或者直接启动项目就可以啦~
