### vue原理-MVVM

vue的作用相当于VM层，进行数据与视图之间的

##### mvm模型

将整个前端页面分成View（视图，页面渲染)，Controller(控件)，Modal(数据源，业务逻辑)，视图上发生变化，通过Controller将响应传入到Model，由数据源改变View上面的数据。

在这个过程中view与Model之间可以直接通信，当两者之间的业务量很庞大时，背离开放封闭原则。因此产生MVVM模型

##### MVVM模型

MVVM分别指View，Model，View-Model，View通过View-Model的DOM Listeners将事件绑定到Model上，而Model则通过Data Bindings来管理View中的数据，View-Model从中起到一个连接桥的作用。

MVVM模型的实现原理可通过Object.defineProperty()与Proxy()实现

#### 数据响应原理

##### Object.defineProperty(obj,key, props)

定义了`obj` 对象的key属性相关属性与操作，ES6支持

~~~javascript
let obj = Object.defineProperty({}, 'count',{
    get(){//获取对象属性值时调用
    	console.log(111);
        return this.value;
    },
    set(v){//对象属性赋值时调用
		console.log(222);
        this.value = v;
    }
})
obj.count = 5;//输出222
console.log(obj.count)//先输出111，再输出5

~~~

属性的set()函数中，当每次数据改变时都会被调用，故可将数据在页面上的渲染的函数在此处调用，就可以实现页面随着数据变化。这就是MCCM的实现原理。

##### Proxy()代理

定义基本操作的自定义行为。ES6支持

语法：new Proxy(target, handler);

* target  用`Proxy`包装的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。
* handler  一个对象，其属性是当执行一个操作时定义代理的行为的函数。

~~~javascript
//类似于defineProperty
let product = new Proxy({},{
    get (obj,prop){
        console.log(obj,prop);//{count: 20} "count"
        return obj[prop];
    },
    set (obj,prop,value){
        console.log(obj,prop,value);//obj对象，'count',20
        obj[prop] = value;
    }
})
product.count = 20;
~~~

### OOCSS面向对象的CSS

不同于js的面向对象，它是将css样式中相同的模块编写在一个class名下，在需要的地方定义该样式即可，提高css代码的复用以及可维护性。

OOCSS 的思维模式：

* 避免使用子孙选择器（例如：不要用 .sidebar h3）
* 避免使用 ID 作为样式钩子
* 避免将类依赖于元素 (例如：不要用 div.header or h1.title)
* 除了在少数例子里，避免使用 !important
* 使用 [CSS Lint ](http://csslint.net/)检查 CSS 代码
* 使用 [CSS grids](http://www.stubbornella.org/content/2011/01/22/grids-improve-site-performance/)

### 代码规范

##### BEM：class命名规范

BEM代表块（Block），元素（Element），修饰符（Modifier）。

* 块(block) 将网页分为独立的块，可进行复用，块之间可嵌套
  * 一个项目中的块名必须是唯一的，明确指出它所描述的是哪个块。相同块的实例可以有相同的名字
  * 一个块的class命名就是这个块的名字
  * 使用-连字符连接 块 与 元素 来进行命名（如blobk-elename)
  * 使用模板引擎时每个模块都有自己的模板
* 元素(element)  块中的具有一定功能的元素
  * 一个块范围内的一种元素的名字也必须是唯一的。一种元素可以重复出现多次
  * 使用__连字符连接 元素 与 修饰符 进行命名
* 修饰符(Modifier)  相似的块或元素，当行为和样式存在不同时采用修饰符标识

##### ESLint：js代码规范检查

按照设定的规则对js代码规范进行检查，不符合规则的代码将会报错

##### 参考代码规范：spec

`https://github.com/ecomfe/spec`

### css框架

#### bulma

引入Bilma的css文件即可

#### mint-ui



### 开发环境判断

#### process.env.NODE_ENV

`process.env.NODE_ENV` 是node的一个全局变量，进程环境：development/production

应用于开发过程中使用的baseURL的改变，不同环境下的地址不一样

### ajax和axios、fetch的区别

最常用axios，体积小，提供并发封装

#### fetch

ES6中出现，使用promise对象，原生的js，没有使用XMLHTTPRequest对象

web提供的获取异步资源的api，可省去ajax请求的繁琐，目前还未被所有浏览器兼容，返回一个promise，第一个then()中需要对返回数据进行处理，第二个then()中再操作数据。默认get方法

~~~javascript
// 高级浏览器原生支持的fetch
fetch('https://jsonplaceholder.typicode.com/todos')
    .then(resp => resp.json())
    .then(resp => console.log(resp))
~~~

#### 传统ajax

传统 Ajax 指的是 XMLHttpRequest（XHR），核心使用XMLHttpRequest对象，多个请求之间如果存在先后关系会造成回调地狱

#### JQuery ajax

对传统ajax进行封装，增加对jsonp(解决跨域)的支持。

缺点：针对mvc编程，不符合MVVM模型。jquery项目太大，单纯使用ajax需要引入整个jquery不合理。

#### axios-vue常用

axios 是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端，本质上也是对原生XHR的封装，只不过它是Promise的实现版本，符合最新的ES规范。

vue中推荐使用它来进行数据请求，功能上对比fetch可设置axios实例对象，对象中设置baseUrl。以及突出优势是可以进行请求拦截与响应。提供get与post两种方法

优点：从浏览器中创建 XMLHttpRequest、支持 Promise API、提供了一些并发请求的接口、拦截请求和响应、自动转换JSON数据

##### 安装

采用npm安装到项目中，或者直接引入axios文件

* `npm install axios`
* script标签中引入src="./axios.js"

##### 创建axios实例

~~~javascript
// 创建一个axios的实例
const ajax = axios.create({
    baseURL: 'https://jsonplaceholder.typicode.com'
})

~~~

##### get()/post()

* get()

  ~~~javascript
  axios.get('/user?ID=12345',{
      params: {
        ID: 12345
      }
    })
    .then(function (response) {
      // handle success
      console.log(response);
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
    .finally(function () {
      // always executed
    });
  ~~~

* post()

  ~~~javascript
  axios.post('/user', {
      firstName: 'Fred',
      lastName: 'Flintstone'
    })
    .then(function (response) {
      console.log(response);
    })
    .catch(function (error) {
      console.log(error);
    });
  ~~~


##### 拦截器Interceptors

应用于等待数据加载成功的过程中，给用户等待的效果，如转动的圈

###### 请求拦截器

* 处理每个请求都要带上token
* get或者post方法需要在url或者data上面增加一些默认要带上的数据

~~~javascript
ajax.interceptors.request.use((config)=>{
    //开始发送请求
    //此处给用户等待的提示效果，转圈等待
    console.log("start request")
	return config;
})
~~~

###### 响应拦截器

* 用户加载提示框隐藏
* 处理全局错误

~~~javascript
ajax.interceptors.response.use((resp)=>{
    //请求得到响应，停止转圈
    //还可以进行全局处理数据或者错误
    console.log("response")
	return resp;
})
~~~

##### axios挂载到Vue

```javascript
//在vue应用下可以将axios实例挂载到Vue上
// 把这个实例挂载Vue构造器的原型上
Vue.prototype.$http = ajax

//使用
new Vue({
      el: '#app',
      created () {//vue实例某个生命周期-在实例创建完成后被立即调用。
        // 任意一个vue实例里都可以通过this.$http去调用这个方法了
        this.$http.get('/todos')
          .then(resp => console.log(resp))
      }
    })
```

##### 项目中使用

常在src下面新建request文件夹，新建index文件专门处理axios请求，然后将方法暴露出去。在main.js中引入该index中暴露的方法，存到一个对象上面，再将对象挂载到全局Vue上面，这样所有的Vue实例都可以调用。





### 虚拟Dom

vue会根据Dom构建一个虚拟dom对象，先对虚拟对象进行操作，找到更改点之后再对真实dom进行操作

* 如果在运行过程中把div#bottom的内容做了修改，这个时候vue首先修改 virtualDom 这个对象里的字段
* 将新的virtualDom跟老的virtualDom对比，对比方式用的是diff算法，找到他们之间的区别（脏检查）再根据区别去修改部分的真实DOM

~~~html
<div class="box">
    <ul>
      <li></li>
    </ul>
    <div id="bottom">
      bottom
    </div>
  </div>
  <script>
  // 虚拟DOM基本原理
  var virtualDom = {
    tag: 'div',
    class: 'box',
    style: {},
    children: [
      {
        tag: 'ul',
        style: {},
        children: [
          {
            tag: 'li'
          }
        ]
      },
      {
        tag: 'div',
        id: 'bottom',
        text: 'bottom'
      }
    ]
  }
  </script>
  
~~~



### 



