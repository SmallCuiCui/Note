## react-router-dom

安装`cnpm install react-router-dom -S`

组件内放元素，可以通过this.props.children获取

context上下文对象，

### 路由模式

在index.js引入路由。并注册

~~~react
import {路由模式 as Router} from 'react-router-dom'
ReactDOM.render(
   <Router>	
		<App />
   </Router>, document.getElementById('root'));
~~~

#### history模式

 BrowserRouter  上线过程会遇到问题。。nginx配置

~~~javascript
import {BrowserRouter as Router} from 'react-router-dom'
~~~

#### hash模式

  HashRouter

~~~javascript
import {HashRouter as Router} from 'react-router-dom'
~~~

### 涉及组件

#### Link

相当于vue中的router-link的作用

~~~react
<Link to="/home/one">one<Link/>
~~~

#### NavLink

相比较Link组件来说，多了一个active样式，可以通过active设置样式

~~~react
<NavLink to="/home/one">one<NavLink/>
~~~

#### Route

进行路由匹配，path值为匹配路径，component为匹配到该路径下的组件

不写path值时，表示前面的路径都没有匹配就走这个路由，一般设置404页面，注意要写在最后

~~~javascript
// 直接通过component指定匹配路由的组件，但是这种情况下无法向组件传递参数
<Route path="xxx" component={Home} exact >
// 使用render传递子组件，注意一定要传递props
<Route path="xxx" render={(props)=>{return <myCom {...props} a={5} />}}>
// 未匹配路由
<Route component={NotFound} />
~~~

#### Switch

当路由路径相同的情况下会匹配多个，甚至会匹配到上级路由，使用Switch后只保留匹配的第一个

~~~react
<Switch>
    {/*只会匹配到第一个*/}
    <Route path="/home/one" component={Home} />
    <Route path="/home/one" component={Detail} />
</Switch>
~~~

#### Redirect

将一个路径重定向到另一个路径下，比如"/"定向到"home"，部分电脑重定向可能存在问题

~~~react
<Redirect from="/" to="/home" exact>
~~~

### 子路由

Route的path匹配时采用:来进行匹配

~~~
<Route path="/home/:type" component={List} />
~~~

### 路由传参

componentDidMount

componentDidUpdate

\<Route path="/home:type" component={} />

#### 获取

路由三个重要属性：通过this.props.xxx获取

componentDidUpdate检测数据变化，可以检测到路由的变化

#### location

获取动态路由下的参数

~~~js
// this.props.history.push("/home",{type:type})
this.props.location.state.type
~~~

#### match

它的下面有params，取得路由的参数

~~~js
// this.props.history.push("/detail/"+type+"/"+id)
this.props.match.params.type
~~~

#### history

实现编程式导航

### 编程式导航

实现其他标签的点击事件+编程式导航替代\<RouterLink>  

this.props.history的方法：

* go
* push(path,state)  path为路径，state为参数，通过this.location.state获取
* goBack

## 补充

### classnames

安装:npm install classnames -S

引入:import classNames from "classnames"

使用：className={classNames({"myClass":true})}为true就有myClass的样式，为false就没有

### style-components

安装：npm install 

使用：

~~~react
import styled from "style-components"
//返回的是一个组件，首字母大写
const Div = styled.div`
	background-color:red;
	ul{
		li{
			color:#fff;
		}
	}
`
// 使用时div变成Div

// 重塑组件，继承样式
const Newdiv = styled(Div)`
	font-weight:600;
`
// 事件绑定到元素上面，如果没有释放，可能导致内存泄漏
// vue与react的事件绑定在元素上，组件销毁时事件就移除销毁了
// 并且是采用事件委托的方式实现的事件绑定，故也不会影响加载

~~~

### prop-types

作用：对组件传递的数据props进行验证。不满足验证会发出警告

安装：npm install prop-types

无状态组件：

~~~react
import propTypes from 'prop-types'
// One是一个无状态组件
One.propTypes={
	a:PropTypes.Number.isrequired
}
One.defalutProps={
	a:10
}
// 定义完再暴露出去
export default One
~~~

类组件：

~~~react
export default class Two extends Component{
    // static里面不能使用this,因为还没有挂载
	static propTypes={
		a:PropTypes.Number.isrequired
	}
	static defalutProps={
		a:10
	}
	render(){
		return <div></div>
	}
}
~~~

### dangerouslySetInnerHTML

渲染字符串类型的标签

~~~react
render(){
	const a = "<h1>hello</h1>"
    // 注意__html前面是两个下划线
	return <div dangerouslySetInnerHTML={{__html:a}}></div>
}

~~~

### Fragment

空标签，可避免渲染不必要的div

~~~react
import React,{Fragment} from "react"
render(){
	return <Fragment>
		<p>空标签</p>
		<p>空标签不会渲染</p>
	</Fragment>
}
~~~

### pubsub-js

实现非父子组件传值

可解决问题：将异步的值传递给子组件

安装:npm install pubsub-js、

引入：import pubsub from "pbusub-js"

~~~react
// 组件a
import PubSub from "pubsub-js"
export default class One extends Component{
	send(){
		pubsub.publish("msg",1234)
    }
}
~~~

~~~react
// 组件b--订阅--在constructor中进行监听
import PubSub from "pubsub-js"
constructor(props){
	super(props)
    pubsub.subscribe("msg",(evt,data)=>{
		console.log(data)
    })
}
~~~

### withRouter

作用：给组件添加路由对象，比如为APP组件添加，它不是路由切换的组件，所以本身没有路由对象。

它是一个高阶组件。运用了属性代理

~~~react
// 属于路由的内容，直接引入使用
import {Route,withRouter} from "react-router-dom"
class App extends Component{
    render(){
        // 使用withRouter前输出为空对象，使用后输出就存在路由的三个对象
		console.log(this.props)
        ///...
    }
}

// 暴露的时候使用
export default withRouter(App)
~~~

### 高阶组件

组件的参数是一个组件，并且返回值是一个组件。高阶组件的组件名可以省略，它只使用一次。

* 给路由切换的组件传参
* 做权限判断

#### 属性代理

* 操作props，找一般对props进行查找和增加

  ~~~react
  function HOC(Com) {
    return class extends Component {
      render() {
        // 增加新的props属性
        const newProps = {
          user: currentLoggedInUser
        }
        return <Com {...this.props} {...newProps}/>
      }
    }
  }
  ~~~

* 为组件添加

* 包裹组件

  高阶组件包起来。高阶组件是一个无状态组件，在需要的组件内引入高阶组件，将组件作为参数调用高阶的方法，并返回这个高阶组件的调用

  相当于是把参数组件重新包装之后，再返回包装后的组件

  ~~~react
  // 高阶组件 Hoc
  export const Hoc = (Com) => {
  	return class extends Component{
          render(){
              // 此处的this.props一定要传，否则返回的组件中无法获取到props
              return <div>@right hyc <Com {...this.props} /></div>
          }
      }
  }
  ~~~

  ~~~react
  // 需要进行包装的组件
  import {Hoc} from "../Hoc"
  class One extends Component{
  	render(){
  		return <div><p>one component</p></div>
  	}
  }
  // 得到的是使用Hoc包装过的组件
  export default Hoc(One)
  ~~~

#### 反向继承

高阶组件返回的组件继承了参数组件，手动调用super方法来渲染参数组件。

当高阶组件中定义了与参数组件同名的方法时将发生覆盖。需要调用super.xxx来调用

功能实现类似于，对参数组件进行二次封装。

例：对于组件是否显示需要进行验证，验证通过则调用继承的组件的render

~~~react
// 高阶组件
export const Hoc = (num) => (Com) => {
    // 注意此处继承的是参数组件
	return class extends Com{
		render(){
            return <div>num>10?<p>大于10的情况下返回这个p</p> : super.render()</div>
        }
    }
}
~~~

* 相似概念：高阶函数，函数的返回值是一个函数或对象

函数科里化。

~~~js
var sum = (x) => {
	return (y)=>{
		return x + y
	}
}
// var sum=(x)=>(y)=>x+y
console.loh(sum(1)(2))//3
~~~

### 监听路由变化

this.props.history.listen(callback)可监听路由的变化

~~~react
//监听路由实现更改页面标题
constructor(props){
	super(props)
    this.props.history.listen((location)=>{
        // 将当前路由的路径传递过去，根据不同路由进行不同操作
		this.changeTitle(location.path)
    })
}
changeTitle(path){
	switch(path){
        case "/home":document.title="home";breack;
        //...
    }
}
~~~

### 登陆验证

跨页面传值-localstorage

登陆之后的跳转，可以从this.props.location中获取到过来的路径，也可以通过localstorage保存路径(location.pathname)。使用this.history.goBack()不一定能跳转过去

 vue 的权限验证，然后跳转，需要考虑子路由。看官网文档，路由元信息

























