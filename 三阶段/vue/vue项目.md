#### --open

### UI库-mintui

安装：`npm install mint`

#### 全局引入

~~~
require ('min-ui')
~~~



#### 局部引入

### css样式reset

github

#### sass

npm---Sass Loader

\<style lang='scss'>\</style>

ctrl+d  选中相同内容

### icon字体

### 数据获取

### 轮播

## vueX

作用：全局状态管理

### 特性

* 单项数据流action，view，state
* 适用于大型应用

### 核心概念

```js
// 定义注册全局状态
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})

// 挂载到main.js的实例上
new Vue({
  store,
  render: h => h(App)
}).$mount('#app')

// 任意组件上使用、
// 获取
this.$store.state.count
```

#### state

状态管理，状态获取放在computed中

##### mapState

将state中的数据映射过来返回一个对象，不同于原数据，它得到state里面的属性得到的函数

```javascript
res = mapState(['count'])
//res是一个对象，对象的键值中有count，它是一个函数
```

它的作用是取代computed中获取state中属性的写法，会更便捷

```javascript
// 原来的computed写法
computed: {
    count () {
        retrun this.$store.state.count
    }
}


import { mapState } from 'vuex'
// 使用mapState
computed: mapState([count])
// 当前组件还有自己的属性时
computed: {
    //使用对象在展开运算符
    ...mapState(['count']),
    abc () {
        
    }
}
```

#### getter

状态存在依赖时，即依赖state中的状态，此时放在getter中。同state一样，在组件的computed中获取

getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算

~~~javascript
export default {
    // 第一个参数是state  存在返回值
  totalCartNum (state) {
    return state.cart.reduce((num, product) => {
      num += product.num
      return num
    }, 0)
  }
}
~~~

##### mapGetters

~~~javascript
import { mapGetters } from 'vuex'
{
    computed: {
        ...mapGetters(['totalCartNum'])
    }
}
~~~



#### mutation

唯一可以修改state里面的状态的地方，设置参数时可以使用对象解构，为参数预定义一个初始值，避免没有传参出现错误

~~~javascript
// 定义
{
    // 参数为state  参数二是传的参数
    increment (state， { id }) {
      state.count++
    }
}
~~~

~~~javascript
// 组件中使用  提交mutation
this.$store.commit('increment',{id:5})
~~~

##### mapMutations

将mutation中的方法统一commit过来。如果有参数的话，直接在调用的地方传参即可。

~~~javascript
// 原来的写法
methods: {
	decrement () {
		this.$store.commit('decrement')
    }
}
// mapMutations 方法
methods: {
    ...mapMutations(['decrement'])
}
~~~

#### action

异步下面执行mutation，不可以直接commit，需要通过action来提交commit，但它不是直接更改state。

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters

~~~javascript
// 定义action
export default {
    // login作为方法在组件中调用，
  login (context, { username, password }) {
      // postLogin是一个异步的请求，当请求返回之后再提交一个mutation方法，进行修改state
    postLogin({ username, password }).then(resp => {
      if (resp.code === 200) {
        context.commit('toggleIsLogin', { })
      }
    })
  }
}
~~~

~~~javascript
// 组件中使用action的方法
// 以载荷形式分发
this.$store.dispatch('login', { ... })
~~~

##### mapActions

~~~javascript
import { mapActions } from 'vuex'
{
    methods: {
    ...mapActions(['login'])
    }
}
~~~



### 订阅

订阅是在进行提交，或者action

#### 订阅提交

当存在提交时就会触发，根据提交的类型做出动作

~~~javascript
store.subscribe((mutation, state) => {
    // toggleUser是mutation中的一个方法
    if (mutation.type === 'toggleUser') {
        console.log('')
    }
})
~~~

#### 订阅action

~~~javascript
store.subscribeAction((action, state) => {
  console.log(action.type)
  console.log(action.payload)
})
~~~

## 项目知识点

### keep-active

### 自定义指令

#### 返回顶部

可直接写一个子组件引入使用，也可以注册一个插件

### 全局处理

#### main.js

* 全局路由守卫，根据路由中设置的meta值，决定不同的路由下做不同的动作，如：
  * 查询是否需要进行登录验证，如果需要，强制路由跳转至登录页面
  * 不同页面下头部组件的名称(可传参实现)，设置为全局state状态，重路由的meta中获取更新

* 全局方法定义，可采用mixin混入，或者定义全局的Vue选项
  * mixin定义：Vue.mixin({})  如定义保留两位小数的过滤器，处理过大的数据(99+)
  * Vue.component('',{})  定义全局组件
  * Vue.directive('',{})  定义全局自定义指令

* 引入路由router，vuex的store，挂载到Vue实例

#### vuex的index

* 引入vuex,使用vuex，新建vuex的store实例，将vuex各项(state,mutation,action,getter)等注册在store实例，并暴露store

  ~~~~javascript
  import ...
  Vue.use(Vuex)
  const store = new Vuex.Store({
    strict: process.env.NODE_ENV === 'development',
    state,
    getters,
    actions,
    mutations
  })
  // 处理订阅
  export default store
  ~~~~

* 进行订阅store的提交，提交就是会改变state状态，那么订阅相当于检测数据变化，然后进行相应动作处理

  * 订阅所有提交，从而检测到购物车的变化，改变localStorage中购物车数据

  * 订阅更新用户数据变化的提交，改变用户保存在localStorage的值

    ~~~javascript
    store.subscribe((mutation, state) => {
      if (mutation.type === 'toggleIsLogin') {
        // 用户信息被修改了
        window.localStorage.setItem('login', JSON.stringify(state.userinfo))
      }
    })
    ~~~

### 登录处理-vuex

全局state中保存userInfo，getter中根据state来获得isLogin状态。

提交登录 => 提交action，从后台获得登录是否成功 => 登录成功则提交改变用户信息的mutation => 改变state中userInfo => 订阅检测到更新用户数据的提交，修改本地localStorage

## 项目上线

使用npm run build生成dis

DNS解析、域名、服务器