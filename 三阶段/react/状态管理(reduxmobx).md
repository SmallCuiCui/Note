## redux状态管理工具

仓库是唯一的，属于单例模式。对比mobx。创建一个公共对象，所有组件都可以访问，但是数据操作是单向的

* 存，取，改---单项数据流
* 安装：npm install redux -S

redux

### createStore()

创建仓库，必须传参，参数是一个函数，用于修改状态的

* reducer是一个纯函数，不能直接修改state，不能调用I/O操作，随机数，时间函数Date()
* state
* action

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

* context的静态方法

  static context-Type = context  可以获得通过context传递的props

  注意在constructor中直接使用第二个参数context

  其他组件中使用this.context

## react-redux

安装：npm install react-redux -S

* 入口文件index.js中引入Provider，并用它包裹App，作用是使所有组件都得到store

  ~~~react
  <Provider store={store}>
  	<App />
  </Provider>
  ~~~

* connect (参数一，参数二)(组件)

  * 参数一是一个函数，映射store的数据到props上
  * 参数二是一个actionCreactor对象，映射actionCreactor的方法到props上，可选
  * 组件是指ui组件
  * connect方法返回容器组件

  ~~~react
  import {connect} from "react-redux"
  // mapState作用是映射store的数据到组件--只映射todo模块的数据中的list数据
  var mapState = (state) {
  	return {
          list: state.todo.list,
          count://映射更多数据，依赖于state.list计算出来的数据
      }
  }
  export default connect(mapState,actionCreactor)(Todos)
  // 全部映射
  // export default connect((state)=>state,actionCreactor)(Todos)
  
  // 只传递一个参数
  // connect(mapState)(Todos)
  // connect(null,actionCreactor)(Todos)
  ~~~

## redux-thunk-异步获取数据

当存在异步数据请求时，使用redux-thunk进行处理。

安装： `cnpm install redux-thunk -S`

对比使用redux_thunk插件与未使用的数据操作：

* view action => reducer =>store => 组件内this.getState()
* view => 中间件 发出动作 => store => 组件内this.getState()

### 在创建store实例时使用：

1. 引入applayMiddleware

   import {applayMiddleware} from "redux"

2. 引入中间件

   import thunk from "redux-thunk"

3. 应用中间件

   createStore(reducer,applayMiddleware(thunk))

~~~react
import {createStore, applayMiddleware} from "redux"
import thunk from "redux-thunk"
import reducer from "./reducer"

var store = createStore(reducer,applayMiddleware(thunk))
export default store
~~~

### 异步数据请求时使用：

在某一个组件的actionCreator中进行数据请求，之前未使用插件时返回的是一个对象，使用插件后返回一个回调函数，函数里面的参数是dispatch，当数据在回调中请求得到后，再dispatch一个携带数据的action

~~~react
import {DATA} from "../../store/actionType"
import axios from "axios"

export default {
	getData(){
		return (dispatch)=>{
			axios.get("http://localhost:4000/list").then(resp => {
				dispatch({
					type:DATA,
                    list:resp.data
                })
            })
        }
	}
}

~~~



##  项目开发流程

1. 确认需求，产品出需求文档
2. 产品根据需求文档出圆形图
3. UI根据原型图出设计稿
4. 交给开发团队，前端，后端
   1. 根据需求划分模块
   2. 根据模块，进行模块估期，然后对项目估期
   3. 根据项目划分责任人
   4. 和后端确认接口，后端出接口文档，前端验收接口文档
   5. 上传项目到代码仓库，开发人员下载项目进行开发，后端没有接口，前端mock后端接口-开发服务器
   6. 后端接口开发完毕，前后端联调
   7. 交给测试团队测试(QA)-测试服务器
   8. 交给安全团队测试
   9. 上线（生产服务器）

## mobx

react项目中

react-app-rewired

customize-cra -D

装饰器 -D

mobx -S

mobx-react -S

* 建仓库，可以建多个仓库  store/store.js

  ~~~react
  import {observable,action} from "mobx-react"
  class Demo {
      @observable a=5;
  	@action
      change= ()=> {
         this.a ++ 
      }
  }
  var store = new Demo();
  export default store
  ~~~

* 挂载仓库到组件,app.js文件中

  ~~~react
  import store from "./store
  import {Provider} from "mobx-react"
  ReactDOM.render(
    <Provider store={store}>
  	<App />
    </Provider>, document.getElementById('root'));
  ~~~

* 组件中使用

  ~~~react
  import {observer, inject} from "mobx-react"
  @inject("store")  //引入仓库
  @observer  // 检测数据变化
  class Test extends component{
      render(){
  		return <div>
              {this.props.store.a}
              <button onClick={this.props.store.change}></button>
          </div>
  	}
  }
  ~~~


## immutable

不可修改对象。只要数据发生变化，将返回一个新的副本。自动产生副本，且采用了结构共享的技术，性能好。

安装immutable

~~~react
// 基础使用
import {Map,is,List} from 'immutable';
var m1 = Map({a:1});
var m2 = m1.set("a",2); //修改产生新的对象
var m3 =Map({a:1});
console.log(m1===m3)
console.log(m1.equals(m3))
console.log(is(m1,m3))
 console.log(m2.get("a"),m1.get("a"))
var l1=List([1, 2]);
 var l2 = l1.push(3);  //修改产生新的对象
console.log(l1.get(0),l2.get(0))
~~~



~~~react
import {Map, is, List} from "immutable"

var initState = Map({
    n:6
})
~~~

分模块下要安装：redux-immutable

import {combineRducer} from "redux-immutable"

fromJS将js对象转为im对象。toJS将im对象转为





插槽的概念用法

