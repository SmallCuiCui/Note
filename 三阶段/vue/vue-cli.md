## vue-cli

安装`cnpm install -g @vue/cli-service-global`

创建一个项目：`vue create hello-world`

## 分页类型

### SPA单页应用

页面跳转-js渲染

vue做移动端项目，很多情况下都采用单页应用(只有一个html，各个组件之间相互切换)

优点：

* 交互良好，用户不需要重新刷新页面，数据获取也是通过ajax异步获取，页面显示流畅
* 良好的前后端分离
* 减轻服务器的压力，服务器只需要提供数据即可

缺点：

* seo难度高（搜索引擎优化：利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名，一种自我营销手段）
* 前进后退管理，不能利用浏览器的前进后退功能--我试了可以呀
* 初次加载耗时多

### 多页应用

页面跳转-返回html

优点：

* 首屏速度快，seo好

缺点：

* 页面切换慢

## 文件引入

### 引入bulma.css文件

采用npm安装之后直接引入，而不是引入bulma.css文件

安装：`npm install --save bulma`

引入：

~~~
<style scoped>
@import url('bulma/css/bulma.css');
</style>
~~~

### 引入src文件夹下的文件

使用`@/`指src文件；如果文件是index可以省略文件名

~~~javascript
//引入所有requests下面index.js文件内所有的export放到ajax对象里
import * as ajax from '@/requests'

//将引入的模块挂载到Vue上面，所有的Vue实例都可以使用
Vue.prototype.$http = ajax
~~~

## 数据请求

### axios

vue中推荐使用它来进行数据请求，功能上对比fetch可设置axios实例对象

~~~javascript
/* equest/index.js /
//npm先安装
import axios from 'axios'
// 创建acios实例
const ajax = axios.create()
// 定义数据请求方法，并暴露出去
export const getHotSearch = () => ajax.get('/module/mget?code=sketch%2ChotWord')
~~~

~~~javascript
// 在main.js中引入数据请求方法，并挂载在全局的Vue上，Vue.property
import * as ajax from '@/request'
Vue.prototype.$http = ajax
~~~

数据请求做法：常在src下面新建request文件夹，新建index文件专门处理axios请求，然后将方法暴露出去。在main.js中引入该index中暴露的方法，存到一个对象上面，再将对象挂载到全局Vue上面，这样所有的Vue实例都可以调用。下面是两种开发情况下的数据请求处理

#### 前后端并行-baseUrl

前后端并行时，后端没有数据，自己模拟的数据使用假数据接口，设置baseUrl，当开发上线之后再改成真实的数据接口。

~~~javascript
//index.js
import axios from 'axios'

// process.env.NODE_ENV 是node的一个全局魔术变量，进程的环境： development\production
const isDev = process.env.NODE_ENV === 'development'
const ajax = axios.create({
    // 定义baseUrl
    baseURL: isDev ? 'https://jsonplaceholder.typicode.com' : '上线之后的真实接口地址'
})
export const getAllTodos = () => ajax.get(baseUrl + '/todos')
// 存在参数，使用模板字符串拼接
export const getById = id => ajax.get(`${baseUrl}/todos/?${id}`)
~~~

~~~javascript
//main.js
import * as ajax from '@/requsets'
Vue.prototype.$http = ajax
~~~

~~~javascript
//组件中使用
 // 常在这个生命周期进行数据请求
created(){
    this.$http.getById(5).then(resp => {
      console.log(resp)
    })
  }
~~~

#### 数据接口已上线-proxy

后端数据已经上线，前端直接使用接口时，在开发状态下可能存在跨域，则采用代理的方式，vue下的代理只需要在根目录下新建一个配置文件(文件名固定就叫vue.config.js)，在里面配置

##### vue.config.js

项目的根目录下新建此文件夹，配置代理的路径。此方法就是使用的npm下面的http-proxy-middleware中间件

~~~javascript
//vue.config.js文件
module.exports = {
    devServer: {
        // 直接写所有代理的源
        //proxy: 'http://apiv2.pingduoduo.com'
        //分开为每个接口写代理源
        proxy: {
            '/api': {
                //代理的数据源地址
                target: 'http://apiv2.pingduoduo.com',
                ws: true,
                changeOrigin: true
            }
        }
    }
}

//前端请求的时候就当做是在进行自己服务器下进行数据请求，然后由后端自动代理到源地址上面，当项目上线之后就不会存在跨域了
~~~

## 路由router

一个完整页面的组件放在view文件夹下，单个完整页面的子组件放在components文件夹下。根目录下定义一个router.js文件进行组件路由的配置，在main.js下引入router并使用。

### 路由使用

#### router.js

引入router插件，新建路由实例，引入所有的路由涉及的组件，并配置路由

~~~javascript
import Vue from 'vue'
import Router from 'vue-router'

import Home from '@/views/home'
import List from '@/views/list'
import Man from '@/components/man'
import Woman from '@/components/woman'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/list',
      component: List,
      children: [
        {
          path: '/list/man',
          component: Man
        },
        {
          path: '/list/woman',
          component: Woman
        }
      ]
    }
  ]
})

~~~

#### router-view

组件中的一个标签\<router-view/>

一个空的容器，渲染当前匹配路由的组件。当路由发生变化时，里面的内容对应路由显示不同的组件内容。

存在多个router-view时需要命名分别进行配置。

#### router-link

路由切换（组件切换），to属性对应router里面的path值，切换router-view渲染的组件。tag属性表示router-link渲染为什么标签。对比动态路由，将to进行属性绑定的不同写法。

~~~html
<router-link tag="li" to="/about">about</router-link>
~~~

#### main.js

引入router.js，并添加该路由到实例下

~~~javascript
import router from './router'
//...
new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
~~~

### 命名路由

组件下有多个\<router-view />时，渲染时需要指定渲染的组件是谁，则需要使用name为\<router-view>命名、未命名的是默认渲染的组件。这样可以应用指定组件(name="footer")在指定页面渲染，而在部分页面不渲染

~~~javascript
{
  path: '/home',
  // 该组件下存在多个组件(router-view)，除了默认组件不命名，其他需要命名，在配置时单独为各个组件配置
  components: {
    default: Home,//默认渲染组件
    footer: MyFooter//渲染命名为footer的<router-view />
    // 在其他组件下不配置footer时就不渲染
  }
}
~~~

## 动态路由匹配

\<router-link :to="">可实现不同组件的跳转，也可以实现只有单个组件，但是请求不同，导致数据内容不同，而得到不同的渲染。:to是一个动态路径参数，它会匹配某种模式下的所有路由，如:to="/user/:id"会匹配所有来自"/user/xxx"的所有路由

#### 路由配置

~~~javascript
//路由配置时，path值不是一个确定的值，由一个':'的动态路径参数，相当于一个占位
{
    path: '/list/:cateName',
    //需要进行命名
    name: 'category',
    //组件只有一个，在该组件中通过$.route获取到请求时的不同参数
    component: Category
}
~~~

#### router-link 

链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象。

* params参数对象中只显示组件名同名属性的值在地址上，其他属性作为参数隐式传递，配合name使用
* query参数时显示所有的传递在地址上面吗，配合path使用

~~~html
<!--组件下<router-link>-->

<!--to属性 提供路由的 name 可以搭配params使用，也开始和query一起使用-->
<router-link tag="li" :to="{name: 'category', params: {cateName: 'man', id: 5}, query:{}}">男士</router-link>
<!--功能类似于 router.push({ name: 'category', params: {cateName: 'man', id: 5}})-->

<!--to属性 手写完整的带有参数的 path 动态组件下获取到的params.cateName='woman'-->
<router-link tag="li" :to="{path: '/list/woman', query: {id: 2}}">女士</router-link>
~~~

#### 参数获取

在该动态组件内，通过this.$route上的参数，获取到name,path,params,query等属性，然后再根据获取到的参数进行数据请求。

~~~javascript
// 根据动态路由的参数进行数据请求
created () {
  const id = this.$route.params.cateId
  // 根据id请求数据
  this.getData(id)
},
beforeRouteEnter (to, from, next) {
  //在这里无法使用this,因为还没有进入这个组件
  // next参数可以传一个回调函数，这个回调函数在next跳转之后执行，回调里的第一个参数就是this
  next(vm => {
     console.log(vm)
     vm.cateName = to.params.cateName
     vm.getData()
  })
}
~~~

## 导航守卫

应用场景：当进行路由跳转时，需要进行某些状态的判断，比如登录状态等。当状态满足时允许跳转，不满足时进行其他跳转等。主要对守卫方法(前置守卫，解析守卫，后钩子)的from,next,to三个参数进行处理。

* from  路由，原路由
* to   路由、目标路由
* next()  函数  即进行路由跳转，可传递参数，如next('/login')，跳转到login路由。无论状态如何，守卫方法内一定要执行next()才会跳转

#### 全局导航守卫

这个会应用于所有的路由跳转上面，因此在main.js文件内进行配置

##### 全局前置守卫

`router.beforeEach((to, from, next)=>{})`

~~~javascript
import router from './router'
// 模拟一个登录状态
const isLogin = true
router.beforeEach((to, from, next) => {
  // meta 路由上默认有的属性，需要配置
  if (to.meta.isAuthRequired === true) {
    if (isLogin === true) {
      next()
    } else {
      next('/login')
    }
  } else {
    next()
  }
})
~~~

###### 元信息meta

meta是路由上面的一个对象属性，在定义路由时可在该对象上自定义标志属性，比如 isAuthRequired用于标记是否需要进行登录验证，当进行导航守卫时的利用该属性进行跳转

~~~javas
{
	path: '/home',
	conponent: 'Login',
	// meta 元信息：当前路由默认就有的一个属性
    meta: {
      // 首页不需要登录验证
      isAuthRequired: false
    }
}
~~~

##### 全局解析守卫

`router.beforeEach((to, from, next)=>{})`，与前置守卫类似，触发在所有组件内守卫和异步路由组件被解析之后

##### 全局后置钩子

`router.afterEach((to, from)=>{})`不会改变导航本身，它没有next()

#### 路由独享的守卫

直接在路由配置时定义`beforEnter`守卫。

~~~javascript
{
	path: '/login',
	conponent: 'Login',
	beforeEnter: (to, from, next)=>{
        //...
	}
}
~~~

#### 组件内的守卫

在路由组件内直接定义下面三种导航守卫：

* `beforeRouteEnter`进入路由之前，该组件实例还未创建。

* `beforeRouteUpdate`  组件路由变化时触发，比如动态组件的路由变化。

* `beforeRouteLeave`

  ~~~javascript
  const Foo = {
    template: `...`,
    beforeRouteEnter (to, from, next) {
      // 在渲染该组件的对应路由被 confirm 前调用
      // 不！能！获取组件实例 `this` 因为当守卫执行前，组件实例还没被创建
      next(vm => {//代替this，vm就是指的this
        console.log(vm)
      })
    },
    beforeRouteUpdate (to, from, next) {
      // 在当前路由改变，但是该组件被复用时调用
      // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
      // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
      // 可以访问组件实例 `this`
    },
    beforeRouteLeave (to, from, next) {
      // 导航离开该组件的对应路由时调用
      // 可以访问组件实例 `this`
    }
  }
  ~~~

#### watch实现导航守卫功能

~~~
watch: {
	"$route" (newVal, oldVal) {
		console.log(newVal)
	}
}
~~~

## 路由相关

#### 重定向

将一个请求地址重定向到另外一个地址，如将'/'重定向为'/home'。重定向后地址栏显示的是重定向的地址，而不是发出请求的地址。

~~~javascript
export default new Router({
  routes: [{
      path: '/',
      redirect: '/home'
    },
    {
      path: '/',
      component: Home
    }]
})
~~~

#### 别名alias

将'/a'的别名设置为'/b'，当用户访问'/b'的时候，世纪上访问的是'/a'，地址栏显示的仍然是'/b'

~~~javascript
export default new Router({
  routes: [
    {
      path: '/home',
      component: Home,
      // 当访问'/other'时会自动访问'/home'
      alias: '/other'
    }
   ]
})
~~~

#### 地址模式mode

单页面路由下的组件跳转，地址上会自动添加/#作为标识，如请求''/home'时地址将渲染为'/#/home‘

要去掉/#需要在路由配置的前面添加mode属性为history

~~~javascript
export default new Router({
  mode: 'history'
  }）
//这种做法下，后端将会作出一些特殊的处理，避免将前端组件路由的跳转识别成请求
~~~

#### 激活组件样式

点击列表链接，实现该链接样式的不一样，一般采用下面两个class进行样式添加

~~~css
.router-link-exact-active,
.router-link-active {
  color: #e00;
}
~~~

#### 限制组件样式

\<style scoped>\</style>

scoped: css样式只对当前组件生效 ，它会为当前组件的标签上额外添加一个样式，使当前样式内容只有在这个样式下才能使用

#### this.$router

this.$router指全局的路由，任何组件都可以调用它的push(),go()方法进行跳转

##### this.$router.push()

这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

* 声明式：`<router-link :to="...">`

* 编程式：router.push()

  ~~~
  //push
  this.$router.push('/')
  this.$router.push({ name: 'user', params: { userId: '123' }})
  ~~~

##### this.$router.replace()

与push方法类似，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

* 编程式：`<router-link :to="..." replace>`
* 编程式：router.replace()

##### this.$router.go(n)

参数是一个整数，意思是在 history 记录中向前或者后退多少步。

~~~javascript
// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)
~~~

#### this.$route

指当前的路由对象，可以获取其中的name,path,params,query等属性。

在动态路由下面\<router-link :to="{path:'',query:'',params:{,}}">

### 路由懒加载-异步路由

当打包构建项目时，一次性打包会导致JavaScript包很大，因此是路由加载时需要该组件再加载对应组件。

处理：在组件引入时，改变一下组件的引入方式

`webpackChunkName`相同的会打包在一个文件内

~~~javascript
//路由配置文件内
const Home = () => import(/* webpackChunkName: "group-foo" */'@/views/Home')
const List = () => import(/* webpackChunkName: "group-bar" */'@/views/List')
const Login = () => import(/* webpackChunkName: "group-bar" */'@/views/Login')
const Category = () => import(/* webpackChunkName: "group-foo" */'@/components/Category')
~~~

## vueX

## UI

* bulma
* mint-ui  移动端
* alement  pc端



