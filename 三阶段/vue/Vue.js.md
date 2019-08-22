Vue代码提示：vetur

## vue实例

#### new Vue

new一个vue对象，指定id，模块组件，一个组件对应一个vue

~~~javascript
const app = new Vue({});
~~~

#### 参数

##### el:挂载点

```htm
el:"#app"
```

##### data:数据

```html
data:{
	a:"1"
}
```

##### methods:方法

~~~
methods:{
        change:function(){
        this.message = this.message.split('').reverse().join('');
    }
}
~~~

##### computed：计算属性

是一个方法，必须有return，返回值就是这个计算属性的值，名称可以直接作为一个data来使用。当依赖的数据发生变化时重新计算。

当数据存在依赖时使用。

```javascript
computed: {
     unCompletedTodos () {  // 每一个计算属性都是一个方法，return的值就是这个属性值
          return this.a+this.b
	}，
    //数据双向改变时
	username:{
		get(){
		//每次取值的时候调用
           return this.xing + this.ming;
        },
        set(value){
		//每次设置值得时候调用，value为设置值
            this.xing = value.slicez(0,1);
            this.ming = value.slice(1)
        }
    }
 }
```

##### watch：监听

监听data中某一个数据，当发生改变时触发的事件。可实现computed的功能，computed更常用

```javascript
watch: {
     //两个参数，第一个为新的值，第二个为旧的值
    //监听data中的username值，此处写成一个方法
	username(newVal, oldVal) {
        console.log(newVal, oldVal)
    },
    //监听路由变化，然后调用方法change（change为当前组件的方法）
    $route: 'change'
}

```

##### filters:过滤器

将data中的原始数据处理之后再返回。应用如根据状态码返回状态，根据价格返回保留两位小数的价格

* 局部注册，只有该vue实例可以使用

```javascript
//price为原始值，真正显示的数据是tofix返回的值
{{price | tofix}}

filters: {
        // 定义一个过滤器，接收要过滤的值。返回保留两位小数的原始值
        tofix (value) {
          return value.toFixed()
        }
      }
```

* 在全局注册filter，整个项目的任意位置都可以使用

~~~javascript
// 全局注册一个filter
Vue.filter('my-filter', function (value) {
  // 返回处理后的值
})
~~~

##### components：组件声明

下面细讲



#### 实例

```html
<!--HTML  id对应vue中的el -->
<div id="app">
    <h1>{{message}}<button v-on:click='change'>反转数据</button></h1>
    <p v-if = 'show'>vue小demo</p>
    <ul>
        <li v-for = "item in shops">
            {{item.id}} 商品名：{{item.name}}  价格：{{item.price}}
        </li>
    </ul>
</div>
```

```javascript
//js
var app = new Vue({
    el:'#app',
    data:{
        message:'hello Vue.js',
        show:true,
        shops:[{
            id:'1',
            name:'鼠标',
            price:50
        },{
            id:'2',
            name:'键盘',
            price:50
        }
              ]
    },
    methods:{
        change:function(){
            this.message = this.message.split('').reverse().join('');
        }
    }
}）
```

#### 模板语法{{}}

双括号(也叫差值表达式)中可以写JavaScript表达式(存在返回值的) 如：{{ 1+1 }}，它不会解析标签

{{message}}  在data中去找该属性，存在则显示，不存在会报错not define

## v-指令

#### v-html

`所有指令的值都是javascript表达式`

由于{{ }}不会解析标签，传输HTML内容需要使用v-html，会将内容解析为html标签，并覆盖掉原标签的内容

```html
<div v-html="'<h5>title</h5>'"></div>
<!--指令内必须是javascript代码，否则会报错，故上面指令内的代码需要用单引号引起来-->
<div v-html="title"></div>

<script>
	const app = new Vue({
		el:'#app',
        data:{
            title:'<h5>title</h5>'
        }
    })
</script>
```

#### v-text

内容原样输出到标签内，不会解析html标签，覆盖原内容

#### v-pre

跳过需要渲染的数据，调试的时候用。

#### v-for

遍历data数据，in或者of都可以，方法都类似，都可以使用()得到值与索引。for-in用的较多

```html
<!--遍历data中的数组list-->
<li v-for="item in list">{{item.title}}</li>
<li v-for="(item,index) in list">{{index}}{{item.title}}</li>

<!--遍历data中的对象person-->
<li v-for="value in person">{{value}}</li>
<li v-for="(value,key,index) of person">{{key}}:{{value}}</li>

<!--遍历data中的字符串str-->
<li v-for="char in str">{{char}}</li>

<!--遍历数字，从1开始到10-->
<li v-for="num in 10">{{num}}</li>
```

#### v-if / v-else

判断条件为true时显示，为false时不显示(直接不渲染)；切换条件时会触发销毁/挂载组件

```html
//v-if与v-else必须是相邻的两个标签，否则v-else不生效
<b v-if="islike === true">喜欢</b>
<b v-else="islike">不喜欢</b>
```

#### v-show

通过控制display属性来进行元素的显示与不显示，不显示时也存在于DOM中，适用于频繁显示隐藏的元素。不同于v-if的直接不渲染与渲染

```html
<b v-show="islike">弹框显示</b>
```

#### v-bind

###### v-bind:属性名  

绑定数据作为标签的属性值   可简写为   :属性名

~~~php+HTML
<img v-bind:src="person.img" :alt="person.name">
~~~

###### :style样式绑定

与原生标签上的style共存

~~~html
divCss:{
	width:"200px",
    height:"200px",
    backgroundColor:"res"
}
<div :style="divCss"></div>
~~~

###### :class按照数据设置

注意双引号内是JavaScript代码，如果直接写字符串，需要使用单引号引起来

* 直接设置class

  ~~~html
  <!--className为data中的一个属性-->
  <span :class="className">{{item.name}}</span>
  ~~~

* 单个class-只有一个取值时

  ~~~html
  <!--根据item.status的真假来判断class是否存在className值-->
  <span :class="{'className':item.status}">{{item.name}}</span>
  ~~~

* 单个class-两种可能取值

  ~~~html
  <!--根据item.status的真假来判断class取status还是nostatus-->
  <span :class="item.status?'status':'nostatus'">{{item.name}}</span>
  ~~~

* 多个class，存在原生写法时，共存

  ~~~html
  <span class="state" :class="className">{{item.name}}</span>
  ~~~

* 多个class，数组设置

  ~~~html
  <!--class取值按照数组中每一项取得-->
  <span :class="['state',item.status ? 'status' : 'nostatus']">{{item.name}}</span>
  ~~~

#### v-cloak

vue实例化的时候这个属性自动消失，直接添加到元素上，没有值。配合css给含有这个属性的元素不显示，可以解决vue实例化慢与dom加载情况下出现的{{...}}的情况。

```html
<div id="app" v-cloak>{{text}}</div>
<style>
    [v-cloak]{
        display:none;
    }
</style>
```

#### v-once

只渲染元素和组件一次。在随后的的渲染下，该元素或组件及其所有的子节点都将跳过渲染。

#### v-model

表单控件或组件上创建双向绑定。input,select,textarea

修饰符：

* .lazy  当绑定在input上时，失去焦点时才触发，而不是改变就触发
* .number  输入字符串转为有效的数字
* .trim  输入首尾空格过滤

## 表单双向数据设置

数据驱动表单数据，表单数据更新数据

#### :value与@input实现

~~~html
<!--使用v-bind使数据绑定到input，使用@input事件，或者@keyup从input绑定到数据-->
<input type="text" :value="inputVal" @input="onInput">

<script>
    const app = new Vue({
		el:'#app',
        data:{
            inputVal:'123'
        },
        method:{
            onInput(e){
                const value = e.target.value;
                this.inputVal = value;
            }
        }
    })
</script>
~~~

#### @change与:checked实现

v-model实现复选框的原理，@change将复选框的选择变化绑定到数据，使用:checked实现数据绑定到checked属性

~~~html
<div id="app">
   <label><input type="checkbox" @change="onchange" :checked="likes.includes('唱K')" value="唱K">唱K</label>
  <label><input type="checkbox" @change="onchange" :checked="likes.includes('打球')" value="打球">打球</label>
  <label><input type="checkbox" @change="onchange" :checked="likes.includes('逛街')" value="逛街">逛街</label>
  <label><input type="checkbox" @change="onchange" :checked="likes.includes('看电影')" value="看电影">看电影</label>
  <p>{{likes}}</p>
</div>    
<script>
    const app = new Vue({
        el:"#app",
        data:{
            likes:[
                "逛街"
            ]
        },
        methods:{
            onchange(e){
                let value = e.target.value;
                if(this.likes.includes(value)){
                    this.likes = this.likes.filter(item=>{
                        return item !== value;
                    })
                }else{
                    this.likes.push(value);
                }
            }
        }
    })
</script>
~~~

#### v-model实现

直接使用v-model实现双向绑定，修饰符trem(去除前后空格),number(转为数字),lazy(失去焦点触发)

~~~html
<input type="text" v-model="inputVal">
<input type="checkbox" v-model="ischecked">
~~~

v-model绑定多个checkbox

~~~html
<label><input type="checkbox" v-model="lickes" value="唱K">唱K</label>
<label><input type="checkbox" v-model="lickes" value="蹦迪">蹦迪</label>
<label><input type="checkbox" v-model="lickes" value="逛街">逛街</label>
<label><input type="checkbox" v-model="lickes" value="看电影">看电影</label>

<script>
    const app = new Vue({
		el:'#app',
        data:{
            likes:[
                "唱K"
            ]
        }
    })
</script>
~~~

## 数据响应

采用下标数组数据的某一个数据时，数据能修改，但是无法更新界面。

改变对象已有属性可以更新视图，但是新增属性也无法更新(全局set方法解决，this.$set()不好使)

~~~js
changeOne(index){
    this.list[index] = "aaa"; // 单独使用无法是页面更新
    this.$forceUpdate()
    
    this.splice(index,1,"aaa")
    
    this.$set(this.list,index,"aaa")
    Vue.set(this.list,index,"aaa")
    // 新增属性name
    Vue.set(this.obj,"name","name")
}
~~~

解决方案： 

* 直接在数据更新之后调用this.$forceUpdate()方法，强制刷新
* 使用this.list.splice()
* 使用Vue.set()//this.$set()

## 事件

#### v-on:事件类型

v-on:事件类型  绑定指定事件，值是JavaScript的表达式或者方法。v-on:可以简写为@

```html
<!--表达式-->
<button v-on:click="isShow = !isShow">点击</button>

<!--方法  在vue中自定义methods对象中的changeShow方法-->
<!--事件处理方法没有加括号时，methods中定义的方法第一个参数默认为事件对象e-->
<button v-on:click="changeShow">点击</button>

<!--事件处理方法加加括号时，可以任意传参，需要事件对象时传递$event-->
<button v-on:click="changeShow(123,$event)">点击</button>

<!--v-on:简写为@-->
<button @click="changeShow">点击</button>

<script>
	const app = new Vue({
		el:"#app",
        data:{
            isShow:false
        },
        methods:{
            //方法中使用data的变量需要使用this来获取，this指向当前这个vue实例
            //this.data
            changeShow(e,event){
				this.isShow = !this.isShow;
            }
        }
    })
</script>
```

#### @input

表单内容变化时触发，可绑定一个methods内的函数，在函数内改变数据

#### @change

checkbox选中状态改变时会触发。绑定methods内的函数来处理数据的变化

#### @keyup

#### nextTick

在进行异步请求数据时使用。nextTick()在下一次Dom循环结束之后调用，返回的是一个promise

~~~javascript
$http = axios.create({
            baseURL:'https://jsonplaceholder.typicode.com'
        });
$http.get('/todos')
    .then(resp=>{
        //获得数据
        this.lists = resp.list
        // nextTick()在下一次Dom循环结束之后调用，返回的是一个promise
        Vue.nextTick()
            .then(() => {
            	this.initSwiper()
    	})
	})

~~~



#### 事件修饰符

修饰事件触发的情况；使用如：@submit.prevent 实现触发提交事件时阻止默认事件  

* .stop阻止冒泡

* .once触发一次

* .prevent阻止表单的提交默认事件

* .capture在捕获阶段进行事件处理，先父级处理再自己处理

* .self只有事件源是自己时才触发

* @keyup.enter键盘的修饰符，点击回车时触发，可写多个，如：@keyup.enter.space

  几个特殊的组合键：.ctrl，.alt，

## ref 获取原生Dom

### ref="xxx"

给dom元素添加ref属性，在vue中通过this.$refs.属性值获取该Dom元素。指定绑定命名一个固定值

~~~html
<input type="text" 
       placeholder="待办事项名称"
       v-model="inputValue"
       ref="addInput"
       @keyup.13="addTodo"
>

//获取dom并操作dom，光标聚焦
this.$refs.addInput.focus()
~~~

### :ref="xxx"动态绑定

:ref在遍历时根据遍历值，动态绑定元素。这样得到的ref才是不一样的，才能标识元素

~~~vue
<ul>
    <li v-for="(item,index) in list">
        <span @click="getNode(index)" :ref="index">{{item}}</span>
    </li>
</ul>
~~~

### 标识子组件

~~~vue
<Com ref = "text"></Com>
// 父组件可以直接通过this.$refs.text.xxx获取子组件的方法或者数据
~~~

## 插槽slot

### 原slot-将废弃

Com组件，存在两个插槽，一个未命名，默认插槽，另一个命名

~~~js
var Com = {
      data(){
        return{
          str: "点击"
        }
      },
      template:`<div>
          <div>组件Com</div>
          <slot></slot>
          <slot name="a" :str="str"></slot>
      </div>`
    }
~~~

组件使用时：为插槽内容指定slot字段，指定显示在对应的slot中。

插槽内容可以直接使用该组件从父组件绑定来的数据，如add方法。但要是用该组件的数据时需要用slot-scope绑定props，并且

~~~vue
<Com :add="add">
   <div>插槽内容b 默认插槽中显示</div>
   <button slot="a" slot-scope="props" @click="add(props.str)">{{props.str}}</button>
</Com>	
~~~

### v-slot

使用v-slot:xxx 取代slot来指定插槽位置，xxx对应slot的命名，但是插槽内容需要使用template标签包裹。`v-slot:`可简写为#

数据传递直接给v-slot:xxx = ""传递

~~~vue
<Com :add="add">
    <template #default>
        <div>插槽内容b 默认</div>
    </template>
    <template v-slot:a='{str}'>
        <button @click="add(str)">{{str}}</button>
    </template>
    <template #p>
        <p>插槽v-slot:可简写为#</p>
    </template>
</Com>
~~~

## mixin混入

将Vue组件中可复用的功能写在一个混入对象中，在全局或者需要的组件中混入即可使用。混入对象可以拥有任意组件选项，组件与混入对象含有同名选项时将以恰当的方式合并，若选项中存在冲突时保留组件内容。

~~~javascript
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
~~~

### 局部混入mixins

注意采用mixins混入的钩子函数，会先一步比组件自身的钩子函数执行。如相同的created，mixins的会先执行

~~~javascript
var vm = new Vue({
  mixins: [myMixin],
  created () {
	//...
  },
  methods: {
    bar: function () {
      console.log('bar')
    }
  }
})
~~~

### 全局混入mixin

~~~javascript
// 可应用全局封装好的ajax
Vue.mixin(myMixin)
~~~

## 自定义指令

当对普通 DOM 元素需要进行底层操作，这时候就会用到自定义指令

### 注册

钩子函数：

* inserted  被绑定元素插入父节点时调用，最常用
* bind

inserted钩子函数的参数

* el 指令所绑定的元素，可用于直接操作DOM
* binding  一个对象，包含以下属性
  * name  指令名，不包括-v
  * value  指令绑定值，可以是一个直接的值，也可以是一个对象
  * express指令绑定值的字符串形式，如自定义指令v-demo="message"，message的值为hello。那么express值为message，value值为hello
  * arg  传给指令的参数，如自定义指令v-demo:foo，foo就是参数
  * modifiers  指令修饰符对象  如v-demo.bar.foo，修饰符对象为{ bar:true, foo: true }

#### 全局注册

~~~javascript
//全局定义自动获取焦点的指令v-focus
Vue.directive('focus', {
    inserted (el, binding) {
        // el是使用指令时挂载的元素
        // bingding是在指令上面绑定的数据，可以直接得到一个数据或者一个对象
        el.focus()
    }
})
~~~

#### 局部注册

~~~javascript
{
    data: {},
    directives: {
        focus: {
            inserted (el, bingding) {
                //inserted 被绑定元素插入父节点时调用
                el.focus()
            },
            bind () {
                 //bind 只调用一次，指令第一次绑定时调用
             }
      	}
    }
}
~~~

### 使用

~~~html
//使用
<div v-focus="{name:'', key: ''}"></div>
<div v-focus="value"></div>
~~~

## 生命周期

vue实例下不同时期获取实例的相关内容输出进行比较

~~~html
<div id="app">
   <h1 ref="title">{{ msg }}</h1>
</div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        msg: 'hello world'
      },
        //不同时期输出
      beforeCreate(){
          //undefined undefined undefined
        console.log(this.$el, this.$data, this.$refs.title)
      },
      created () {
          //undefined  Object{} undefined  在这里获取到了自己的data,创建完成
        console.log(this.$el, this.$data, this.$refs.title)
      },
      beforeMount () {
          //<div id="app"><h1 ref="title">{{ msg }}</h1></div>  
          //Object{} 
          //undefined  获取到了data,挂载元素，但是没有渲染数据
        console.log(this.$el, this.$data, this.$refs.title)
      },
      mounted () {
          //<div id="app"><h1 ref="title">hello world</h1></div> 
          //Object{} 
          //<h1>hello world</h1>  获取到了data,挂载元素，但是没有渲染数据
        console.log(this.$el, this.$data, this.$refs.title)
      },
      beforeUpdate () {
          //数据更新渲染已经完成了
        console.log(this.$el, this.$data, this.$refs.title)
      },
      updated () {
           //数据更新渲染已经完成了
        console.log(this.$el, this.$data, this.$refs.title)
      }
        //后面销毁的效果
    })
</script>
~~~

### 创建

#### beforeCreated

没啥用，为创建做准备

#### created

创建了vue实例，但是并未与dom关联。一般在这里进行ajax请求

### 关联

#### beforeMount

挂载之前，拿到容器元素，但是数据还没有往元素上面渲染，这里一般不会做太多操作。

注意：此钩子中数据已经改变，只是页面还未渲染

#### mounted

关联dom，挂载dom完成，渲染dom完成

### 更新

#### beforeUpdate

没啥用，一般不会做什么，

#### updated

完成数据更新，对更新做一些其他响应就在这里做

数据改变时，更新虚拟dom，再渲染真实dom

### 销毁

#### beforeDestroy

即将销毁，清除无用的定时器，移除一些无效的事件监听等

#### destroyed

### keep-alive

当组件使用keep-alive后，不存在销毁阶段的钩子，由下面两个钩子取代

#### activated

keep-alive组件激活时调用，在服务器端渲染期间不被调用

#### deactivated

keep-alive组件停用时调用

### errorCaptured

当捕获到一个来自子孙组件的错误时调用，接收三个参数：错误对象，发生错误的组件实例，错误信息来源的字符串。

此钩子返回false可以阻止错误继续向上传播。模板或渲染函数中设置短路条件可以避免错误的发生。