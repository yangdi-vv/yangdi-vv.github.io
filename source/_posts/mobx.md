---
title: Mobx 基本使用
categories:
- Mobx
- Vue
- React
tags:
- Mobx
---

## Mobx简介
> mobx 是一种简单可扩展的状态管理工具，
> 核心原理是所有应用状态的部分都可以自动获取状态变化，
![运行流程图](http://101.34.242.163/img_blog/mobx.png)

## Mobx关键点
- action
- obeservable
- makeObservable
- makeAutoObservable
- observer

> action：
> 添加状态修改的API

> observable
> 添加需要观察的状态

> makeObservable
> 绑定当前this下的属性为可观察的对象

> makeAutoObservable
> 同makeObservable,自动转化this下的属性可观察

- reaction

- computed
> 计算观察属性

- Autorun
```js
import {action, observable} from 'mobx';

class MobxExample {
    constructor() {
        makeAbservable(this);
    }
    
    @observable public name = 'jack';
    @observable public age = 12;
    
    @action.bound
    setName() {
        this.name = 'john';
    }
    
    @computed get count() {
        return `${this.name}-${this.age}`;
    }       
}
```

- observe
```js
import React  from 'react'
import {makeAutoObservable} from "mobx";
import {observer, inject} from "mobx-react";

class ReaCom {
    private num: number = 0;
    constructor() {
        makeAutoObservable(this)
        this.num = 0;
        setInterval(() => {
            this.num ++;
        }, 1000)
    }
    reset(num: number) {
        this.num = num;
    }
}
const Rea = observer(({reaCom}: any) => {
    console.log(reaCom);
    return <span onClick={() => {
        reaCom.reset(7);
    }}>click {reaCom.num}</span>;
});

class App extends React.Component<any, {Test: any}>{
    constructor(props: any) {
        super(props);
    }
    render() {
        return <Rea reaCom={new ReaCom()}/>;
    }
}

export default App
```

## Mobx-React
> 方便在react中使用mobx
- Provider
```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import {Provider} from "mobx-react";
import stores from './store';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
      <Provider {...stores}>
          <App />
      </Provider>
  </React.StrictMode>
)
```
- inject
```js
import React, {Components} from "react";
import {inject} from "mobx-react";

@inject("MobxValue")
class App extends Components {
    constructor() {
        super(props);
    }
    
    render() {
        return <div>{this.props.MobxValue.name}</div>
    }
}
```
## Mobx-Vue
> 官方推荐react使用mobx,Vue也可使用mobx，但是最好还是使用Vuex更方便。
