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
<Route path="xxx" component={Home} exact >
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

#### 

































