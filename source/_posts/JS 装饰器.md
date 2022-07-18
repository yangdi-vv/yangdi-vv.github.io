---
title: JS 装饰器
categories:
- JS
tags:
- JS
- decorators
---

## 简介
- 装饰器是JS的一种特殊语法，以@符号为前缀，放在我们需要装饰的代码前，并且可以使用多个装饰器
  - 示例
```js
@readonly
class Example {
  @log()
  exampleFunction() {}
}
```

- 装饰器分类
  - 装饰器再JS中可以分为三类
    - 函数装饰器，用函数包装函数
    - 类装饰器， 一次应用整个类
    - 类成员装饰器，应用于类的装饰器
> 目前，不能直接在浏览器或者nodeJS运行装饰器，需要使用转义器支持。

- 函数装饰器（高阶函数）
```js
function doSomeThing(name) {
    console.log(`NAME: ${name}`);
}

function loggingDecorator(wraped) {
    return function() {
        console.log('start');
        const result = wraped.apply(this, arguments);
        console.log('end');
        return result;
    }
}

const wraped = loggingDecorator(doSomeThing);

doSomeThing('jack');
// jack

wraped('jack');
// start
// jack
// end
```

- 类装饰器
```js
function log(name) {
    return function(Class) {
        return (...args) => {
            console.log(`name: ${name} args； ${args}`);
            return  new Class(...args);   //Class为默认接收的Calculator类
        }
    }
}

@log('test')
class Calculator {
    constructor() {
        
    }
}

const calculator = new Calculator('String');
// console.log(name: test args：String)
```
              
- 类成员装饰器
  - 可以将装饰器用于类的属性、方法等，且可接收三个参数
    - target 目标
    - name 目标名称
    - descriptor 目标描述
```JS
function readonly(target, name, descriptor) {
  descriptor.writable = false;
   return descriptor;
}

class Example {
  x() {}
  @readonly
  y() {}
}

const myClass = new Example();
myClass.x = 10;  //报错，已设置不可修改
myClass.y = 20;  //报错，已设置不可修改      
```
## 装饰器的使用
> 装饰器可以在JS中以@符号方式使用，放在被装饰的代码前即可，也可以使用多个装饰器
```js
@one
@two
class Example{
    @three('test')
    doSomeThing() {}
}
```
> 当前浏览器并未支持，需要安装babel插件及typescript
  - 安装
> npm install -D @babel/plugin-prosal-decorators
  - 修改.bablerc
```json
{
  "plugins": [
    [
      "@babel/plugin-proposal-decorators",
      {
        "version": "legacy"
      }
    ]
  ]
}
``` 
       
## 总结
> javascript 中添加装饰器的原因主要是为了类与类之间共享属性和功能，
> 且写法简单，不需要书写复杂的代码来为类增加功能，同时更好的了解装饰
> 器也有利于理解代码编程中的代码复用的思维方式，例如React高阶组件~
