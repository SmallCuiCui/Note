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

\<Link>

\<NavLink>  多了一个active样式

\<Route path="xxx" component exact >

\<Switch> 匹配多个的时候，只保留第一个

部分电脑重定向可能存在问题，

\<Redirect from="/" to="/home" exact>

### 子路由



### 路由传参

componentDidMount

componentDidUpdate

\<Route path="/home:type" component={} />

#### 获取

路由三个重要属性：

通过this.props.xxx获取

* history取参

* location
* match  它的下面有params，取得路由的参数

componentDidUpdate检测数据变化的实现

### 编程式导航

实现其他标签的点击事件+编程式导航替代\<RouterLink>  

this.props.history的方法：

* go
* push(path,state)  path为路径，state为参数，通过this.location.state获取
* 

































