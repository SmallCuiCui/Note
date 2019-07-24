## redux状态管理工具

仓库是唯一的，属于单例模式。对比mobx。创建一个公共对象，所有组件都可以访问，但是数据操作是单向的

* 存，取，改---单项数据流

* 安装：npm install redux -S

### createStore()

创建仓库，必须传参，参数是一个函数，用于修改状态的

* reducer是一个纯函数，不能直接修改state，不能调用I/O操作，随机数，时间函数Date()

~~~react
import {createStore} from "redx"
var initState = {
    n: 8
}
// state是前一个状态的数据，action是对数据的操作
const reducer = (state=initState,action) => {
	// ...对state进行操作
    switch(action.type){
        case "DEL":return{
            ...state,
            list://...处理数据
        } ;break;
        case "EDI": ;break;
        default：return state
    }
    
}
const store = createStore(reducer)
export default store
~~~

### store实例的方法

#### getState()

获取state状态值

~~~react
import store form "../store"
console.log(store.getState().xxx)
~~~

#### dispatch()

修改内部state的唯一方法是提交一个action，在组件内进行修改状态

* action是一个对象，并且必须含有type属性，属性值需要大写

~~~react
// 此处提交的action就是reducer中的action
function delAction(id){
	return {
		type:"INCRE",
    	id
    }
}
store.dispatch(delAction(6))
~~~

#### subscribe()

订阅提交，当存在dispatch动作时触发，可用于获取最新的store.state数据

~~~react
store.subscribe(()=>{
	this.setState({
			todolist: store.getState().list
		})
})
~~~

### 分模块

将每个组件的动作，提交动作处理的reducer都分开写，然后在主的reducer中引入合并

* 不同组件存在自己的reducer，故有不同的state值，getState()的时候需要带上该模块的标识，其他两个方法不变

~~~react
import oneReducer from "./components/one/reducer"
import twoReducer from "./components/two/reducer"
import {combineReducers} from "redux"
var reducer = combineReducers({
	one:oneReducer,
    two:twoReducer
})
~~~

~~~react
// 分模块下组件的store.getState()存在不同
// one组件下
console.log(store.getState().one.n)
~~~





## context

上下文对象，提供一种组件之间无需使用props进行数据传递的方式。

* 引入上下文对象，解构得到Provider, Consumer

  ~~~react
  // conten
  import React,{Component, createContext} from "react"

  let {Provider, Consumer} = createContext()
  
  class ContentProvider extends Component{
  	render(){
  		return <Provider value={this.props.obj}>
  			{this.props.children}
  		</Provider>
  	}
  }
  export {
  	Consumer,
  	ContentProvider
  }
  ~~~

* 使用ContentProvider包裹\<App />

  ~~~react
  // 在index.js文件中
  import {ContentProvider} from "./components/ContentProvider"
  
  ReactDOM.render(
  <ContentProvider obj ={5}>
  	<App />
  </ContentProvider>, document.getElementById('root'));
  ~~~

* 其他任意组件中获取obj

  ~~~react
  import React,{Component} from "react"
  import {Consumer} from "../ContentProvider"
  export class Two extends Component{
  	render(){
  		return <div>
  			<h6>two componnt</h6>
  			<Consumer>
  				{
  					(obj)=>{
  						console.log(obj)
  						return <div>{obj.getState().n}</div>
  					}
  				}
  			</Consumer>
  		</div>
  	}
  }
  ~~~

  

### 插槽的概念用法

