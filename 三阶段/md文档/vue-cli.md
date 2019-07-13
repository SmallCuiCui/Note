## vue-cli

### 安装

```
npm install -g @vue/cli
```

### 创建项目

```
vue create 文件名
```

#### 配置项

1、

```
default(babel,eslint) //默认
Manually select features //自定义安装
```

2、

```
Babel //是否要es6转es5
TypeScript //强类型的js
Progressive Web App //web支持
Router //是否需要路由
Vuex //状态管理
CSS Pre-processors //css加前缀
Linter/Formatter //代码规范
Unit Testing //测试
E2E Testing //测试
```

3、

```
Lint标准
一般选ESLint + Standard config
```

4、

```
Lint on save //保存的时候检查
Lint and fix on commit //修改的时候检查
```

5、

```
In dedicated 。。。//新建一个文件存放
In package.json   //放在package.json文件里
```

6、

```
Save this as a.... //将来的项目也这样配置吗
```



### 项目文件信息

#### package.json

配置项

#### yarn.lock

类似于npm，包管理工具

#### babel.config.js

es6转es5

### src

#### main.js

项目总入口

```
import Vue from 'vue'           //引入vue.js文件
import App from './App.vue'		//引入vue文件

Vue.config.productionTip = false

new Vue({
  render: h => h(App),  		  //渲染app组件
}).$mount('#app')	//挂载到index.html里的#app里
```

#### app.vue

```javascript
import HelloWorld from './components/HelloWorld.vue'
//引入组件

export default { 
  name: 'app',
  components: {
    HelloWorld	//注册组件，组件嵌套
  }
}		//导出文件
```

### 执行项目

```
$ npm run serve
```

### 前端路由

#### App.vue

```html
<router-link to="" tag=""></router-link> //相当于a链接，进入哪个路由。tag可以设置该标签为什么标签，默认为a标签

<router-view></router-view> //加载的组件会写到这里面来
```

#### router.js

配置路由

```javascript
import 自定义组件名 from “路径” //引入组件
Vue.use(Router) //使用路由

routes:[
	{
		path:"", //路径
		name:"自定义名", //自定义一个值传入$route.name
		component:"自定义组件名" //组件名
	}
]
```

#### 路由嵌套（动态路由）

**router.js**

```javascript
例：
{
      path: '/list',
      name: 'list',
      component: List,
      children: [
        {
          path: 'man',
          name: 'man',
          component: Category
        },
        {
          path: 'woman',
          name: 'woman',
          component: Category
        }
      ]
    }
```

```javascript
List.vue
<router-link to="/list/man">男装</router-link>
<router-link :to="{ 
   name: 'category', 
   params: {cateName: 'kid', start: 0},
   query: {end: 20}}">
童装</router-link>
//可动态设置路由，name为$route.name，自定义的一个值。
//params为传递的数据，可传多条。并且cateName: 'kid'就相当于path路径的路由。
//query可以在导航上添加参数。


**route指当前路由，router只更多的数据**


router.js
routes: [
    {
      path: '/home',
      name: 'home',
      component: Home
    },
    {
      path: '/list',
      name: 'list',
      children: [
        {
          path: ':cateName',
          name: 'category',
		  components:Category
        }
      ]
    },

//真实案例中只有一个path，：cateName会动态获取list.vue的/list/man的man。路由值在$route.params.cateName里。
```

#### 编程式导航

```html
router.push({ path: '/', query: { id: 3 } })
// 返回到根目录，query可以在导航上添加参数。
```

#### 视图显示

根据<router-view>的name显示不同的组件

```javascript
<router-view></router-view>
<router-view name=“a”></router-view>

routes: [
    {
      path: '/list',
      name: 'list',
      },
      children: [
        {
          path: ':cateName',
          name: 'category',
          components: {
            default: Category,  
			//默认视图不设置name的组件。
            a: Test
			//设置name为a的视图显示的组件。
          }
        }
      ]
    },
```

#### 元信息

```html
mata{} //路由传递的参数
```

#### 别名

```javascript
routes: [
    {
      path: '/home',
      alias: '/',   //别名，都可以进入主页面
      name: 'home',
      component: Home
    },

```

**list.vue**

```html
 <router-link to="/list/man">男装</router-link>
 <router-link to="/list/woman">女装</router-link>
```

**Category.vue**

```html
 <div>
    这是分类页{{$route.name}}
 </div>
```

### 全局导航守卫

当路由发生改变时执行，进入这个钩子函数，这个函数里只有调用了next（）才会跳转。

#### 基础语法

```html
router.beforeEach((to,from,next)=>{
	from:来自哪个路由
	to:去哪个路由
	next（）：跳转，不调用或者传false就不会跳转，可以传参跳到哪个地址去。
	this：undefinde，因为beforEach
})
```

#### 跳转验证

```javascript
router.beforeEach((to,from,next)=>{
	if(to.mate.isAuthrequired){
		if(isLogin){
			next（）
		}else{
			next("/login")
		}	
	}else{
		next()
	}
})
```

### 路由独享守卫

```html
vue组件
router.beforeRouteEnter //进入路由之前
router.beforeRouteYpadate((to,from,next)=>{
	from:来自哪个路由
	to:去哪个路由
	next（）：跳转，不调用或者传false就不会跳转，可以传参跳到哪个地址去。
	to.params.cateName为参数
})  //路由更新之前
```

```html
vue组件
router.beforeRouteEnter((to,from,next)=>{
	next(vw => {
		//next传一个回掉函数，回调函数的参数就是vue实例。
	})	
})  
```

### 懒加载

#### 异步组件

```javascript
const Home = import("./views/home")或者
const Home = () => 
import(/*webpackChunkName：“a”*/，"./views/home")
import(/*webpackChunkName：“a”*/，"./views/list")
//异步加载,第一个参数相同的就会放到一个包里。
```



### 使用sass

#### 安装sass

```html
$ npm i sass-loader node-sass --save
```

#### 使用sass

```html
<style lang="scss">

</style> 
```





