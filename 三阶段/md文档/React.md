## React

### 官网

https://reactjs.org/

### 搭建环境/安装

```
npx create-react-app 文件名
// npx可以不用安装react
```

### 弹射

```
npm run eject
// 自己制定配置
```

### 插件/包

#### VS-ES7

rfc //函数式组件
rcc //类组件

#### prop-types插件

##### 安装

```
npm install -save prop-types
```

使用

```js
**header.js**
import PropTypes from 'prop-types' //引入

exprot default class Header extends Component{

	static propTypes = {
		title: propTypes.string.isRequired
		//定义传过来的值为字符串类型，any为任何类型都可以，并且是必传的。
	}
}
//所有的静态属性前面加static，静态属性即实例属性。自己的实例不能调用，只能自己调用。
```



### 基础引入

```
**src/index.js**
方法一：箭头函数写法
    
improt React from 'react
import ReactDOM from 'react-dom
// 使用ReactDOM的render方法
const app = (props) => {
    return <div title={props.title}>{props.title}</div>
}

ReactDOMrender(
    app({
        title:'abc
    }),
    document.qu....('#root')
)

```

```js
**src/index.js**
方法二：函数式组件写法
    
improt React from 'react
import ReactDOM from 'react-dom
// 使用ReactDOM的render方法
const App = (props) => {
    return <div title={props.title}>{props.title}</div>
}

ReactDOMrender(
    <App title="1902" />,
    document.qu....('#root')
)
// props代表组件的属性
```

```js
**src/index.js**
方法三：类组件写法
    
improt React from 'react
import ReactDOM from 'react-dom 或者
import {render} from 'react-dom'
// 使用ReactDOM的render方法
class App extends React.Component{
    render(){
        return <h1>{this.props.title}</h1>
    }
}

render(
	<App title="123">,
    document.qu....('#root')
)
```

### 注释

```js
{/* */}
// jsx中就是js表达式是在{}里
```

### 模块引入

```js
**compentens/index.js**
    
方法一：    
import Header from ''
export {
	Header
}

方法二：
export { default as Header } from ''
//相当于先去路径寻找文件，再命名。


**App.js**
    import {
    Header
} from './components'
```

### 空标签

```
import React, {Fragment} from 'react'
<Fragment></Fragment>或者<></>
//页面加载时这个标签会消失
```

### 修改数据

```
fn= ()=>{
    this.serState({
        list:...
    })
}
```

### Refs

#### 作用

访问或参加一个React元素

#### 使用场景

1.管理焦点

2.触发强制动画

3.继承第三方DOM库

#### 用法

1.创建Refs

```
export default class TodoInput extends Component {
    '定义名' = createRef() 
    //创建
}
```

2.绑定到需要指向的元素上

```
 <input ref={this.myRef} />
```

3.找到绑定的元素

```
this.myRef.current
```

### 生命周期

#### 挂载阶段

```
constructor()
// 组件内部用到的内容进行初始化。
static getDeruvedStateFromProps()
// 设置state状态的时候，父组件的props也改变
render()
// 渲染阶段
componentDidMount()
// 组件挂载完成以后，一般用于发ajax请求
```

#### 更新阶段

```
render()
```

```js
shouldComponentUpdate()
// 更新阶段判断是否要重新渲染。根据返回true或false来决定
// 有一个arguments参数集，三个值，第一个是接收的props，第二个是组件内部的state，第三个是一个对象。
// 可接收一个props参数。即当前需要渲染的那个元素的数据
```



### Context

不需props可以在组件中传递数据的方法

#### React.createConext

```js
import {createConext} from 'react' //引入

const {
    Provider,
    // 共享的数据
    Consumer: CounterConsumer
    // 使用共享的数据，加属性值是重命名
} = createContext() 

class 组价a extends Conponent {
    constructor(){
        super()
        this.state = {
            count: 100
        } 
        // 全局可共享的变量
    }
    
    render () {
        // 将需要全局共享的资源作为 <Provider> 元素的 value 属性值
        return (
            <Provider value={{
                count: this.state.count,
                increase: this.increase,
                decrease: this.decrease
            }}>
                {this.props.children}
            </Provider>
			//用Provider包裹所有需要共享数据的组件
        )
    }
}

class Counter extends Component {
    render () {
        return (
            <CounterConsumer>
                {
                    // 函数参数就是在 Provider 的 value 中共享的资源
                    (arg) => {
                        const {count} = arg
                        console.log(arg)
                        return (
                            <>
                                <CounterButton type="decrement">-</CounterButton>
                                <span>{count}</span>
                                <CounterButton type="increment">+</CounterButton>
        // 可共享数据的组件用CounterButton标签包裹。如果标签里不只一个元素节点，需要用{（）=》{}}函数表达式把结构return出来。
                            </>
                        )
                    }
                }
            </CounterConsumer>
        )
    }
}
```

#### Context.Provider

在哪些地方可以使用公共数据

#### Context.Consumer

### Hook

#### 作用

可以在不编写class的情况下使用state及其他特性（用于函数组件）

#### 用法

```
import React , {useState} from 'react' //引入

const [count,setCount] = useState(0)
// 返回两个数组，第一个参数是state，第二个参数是setState

```

### redux

#### 定义

js状态容器

#### 安装

```
npm i redux / yarn add redux
```



#### 三大原则

##### 单一数据源

只有一个store里，存储state。

##### state是只读的

唯一改变state的方法是触发（dispatch）action

##### 使用纯函数来执行修改

为了描述action如果更改state，需要编写reduer

#### state

是一个对象，保存状态数据，不能直接修改state对象属性值。

#### action

是一个对象，在action对象中必须包含一个type属性。

指明动作类型，修改状态时所需要使用到的数据通常在action中使用payload携带

#### reducer

是一个函数，关联state和action，reducer是一种规定约束。在reducer中实现action指明的状态更新

#### dispatch

发送 action 到 reducer

#### subscribe

发布订阅，在dispatch内部，把内部的钩子函数自动调用执行。

#### 用法

```js
//创建一个reducers文件夹
import actionTypes from '' //引入actionTypes文件

**cart.js**
const initState =[{},{}] //state初始数据
export default (state = initState,action) => {
    switch (action.type){
        case actionTypes.A:
            return 
            // 通过return返回一个新state，action.payload.id。可以获取传递过来的参数。
    }
    // 根据action函数的类型做不同的事情。
}
```

```js
//创建一个actions文件夹，保管所有的action

**actionTypes.js**
    export default {
	A:C;
	A:B
    // 保存函数
}

**cart.js**
    import actionTypes from './actionTypes'
	//引入自定义的所有action方法

export const increase = id => {
    return{
        type:actionTypes.A,
        payload:{
            id
            // 携带传入
        }
    }
}
// 通过设置为函数的形式传参动态设置id
```

```js
**最外层引入app.js的index.js**

import {createStore} from 'redux'
// 引入redux
import {reducer from './reducers/cart/'
// 引入调用state和action的文件。
const store = createStore(reducer)
// store是reducer实例。store.getState是state的数据。
```

