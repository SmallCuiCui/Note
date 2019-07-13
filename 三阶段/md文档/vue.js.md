# vue.js

## 基础

### 1、vue实例

```ht
new Vue({})
```

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

##### template：模板

```html
template:"<p>123</p>"
```

##### computed：求和

定义函数，用于数据合并，可以当做一个data来使用。

```html
computed: {
          unCompletedTodos ()   // 每一个计算属性都是一个方法，return的值就是这个属性值
            return this.a+this.b
 }
```

##### watch：监听

监听某一个数据发生改变时触发的事件

```html
watch: {
	a (nVal, oVal) {
	console.log(nVal, oVal)
	//两个参数，第一个为新的值，第二个为旧的值
        }}
```

##### components：组件声明

```html
components: {
        guonei:guonei
      }
```

##### filters:过滤器

用于给一个插值表达式里的数据进行更改

```html
{{a | toUpper}}

filters: {
        // 定义一个过滤器，接收要过滤的值，返回过滤的结果
        toUpper (val) {
          return val.toUpperCase()
        }
      }
```

### 2、v- 指令

#### v-html

```html
<div v-html="a"></div> //插入内容，标签会解析
```

#### v-text

```html
<div v-text="a"></div> //插入内容，标签不会解析
```

#### v-on：

```html
//    v-on：事件句柄=“函数名”     简写 @事件句柄=“函数名” 
@input //input标签的change事件
```

##### 修饰符

```html
@click.shop //阻止冒泡
@click.prevent //阻止默认行为
@click.submit.prevent //不再重新提交表单
@click.captute //监听事件时使用事件捕获
@click.once //只触发一次
@click.self //只在本元素触发，子元素不触发
```

##### 键盘事件

```html
@keyup.键盘编码=“”
// 键盘编码处可写键盘编码或者键盘别名
.enter/tab/delete/esc/space/up/down/left/right/
系统修饰符：ctrl/alt/shift/meta
```

#### v-bind：

```html
//简写 ：属性=“”
：class{banner:is}//根据is的布尔确定是否有该类
：class[top,{banner:is}] //默认设置top类，后面判断其他类
```

#### v-if

```html
v-if=“1>2”
//判断true或false，为true才显示该元素，为false则dom结构里不存在该标签
```

#### v-else

```html
 <div v-if="isModelShow">这是一个弹框4</div>
 <div v-else>这事跟弹框4相反的一个显示</div>
//该指令基于上一个兄弟v-if，根据v-if来判断显示或者消失
```

#### v-show

```html
v-show=“1>2”
//判断true或false，为true才显示该元素，为false则display=none
```

#### v-cloak

```html
 <style>
  [v-cloak] {
    display: none;
  }
  </style>

<div id="app" v-cloak>
    {{msg}}
</div>
// 用于优化页面，vue加载好之后该属性就会消失。
```

### 3、表单

#### 1.表单特性

1.复选框中，v-model的值为true，false，可决定复选框是否被选中。

2.复选框中，v-model在data里的绑定数据为[]时，选中会向[]中添加该添加该input设置的value值，如果没设置为null。

3.单选框中。type相同的input同时v-model绑定一个数据，谁被选中数据里的值即为该input的value。

4.select设置v-model后，选中哪个option，v-model的数据就为

#### 2.input修饰符

##### .lazy

```html
<input v-model.lazy="msg" >
// 数据发生改变时，框内的数据同步改变
```

##### .number

```html
<input v-model.number="msg" >
// 转为数字类型
```

##### .trim

```html
<input v-model.trim="msg" >
// 去空格
```

### 4、组件

#### 1.创建组件

```html
var a = {}  //局部组件
Vue.conmponent("组件名"，{}) //全局组件
```

注册组件名使用大驼峰的时候，html中要把大写变成小写，并用-链接。

#### 2.全局组件使用

```html
<div id=app><content></content> </div>
//全局组件直接使用组件名插入挂载点
```

#### 3.局部组件使用

```html
方法1：
components:{
	content //挂载点注册组件
}
<div id=app><content></content> </div>
```

```ht
方法2：
<body>
	<template id="td-header">
    <section class="hero is-primary">
            <h1 class="title">
              董事长的代办事项
            </h1>
      </section>
 	 </template>            //组件内容
  
  	<div id="app">
  		<td-header></td-header> //把id名写入挂载点
  	</div>
  	
  	<script>
  		const tdHeader = {
        template: '#td-header'  //注册组件
      }
  	</script>
</body>
```

#### 4.组件的参数

##### data

```htm
data(){
    return {
            a: ''
          }
}
//组件的数据是一个方法，因为每个组件的数据都是独立使用的，保证方法不共享，return一个对象，html里不能写驼峰，都要把大写变成小写，并用-链接。
```

##### props

```html
props: {
          id: Number,
          title: String,
          hasCompleted: Boolean，
		a:{
			required:true(必传属性)
			default: 0(默认值)
		   }
        }
//从父组件接收的数据，写类型的话可以验证是否为该类型的数据，不是就报错。
```

#### 5.css过渡

##### transition

需要过渡的元素用<transiton name="fn”></transion>包裹。并设置一个name。如果不写name，用v-过渡类名属性会给所有的过渡元素加样式。

##### 过渡类名

```html
.fn-enter{ opacity: 0} //开始过渡时需要插入显示的元素的样式。

.fn-leave {opacity: 1} //过渡结束时需要插入显示的元素的样式。

.news-leave-to {opacity: 0}
// 开始隐藏的元素过渡完时的样式

.news-enter-to{opacity: 1}
// 开始隐藏的元素准备开始时的样式

.fn-enter-active {transition: opacity 3s}
// 开始显示插入的元素的过渡状态过程

.fn-leave-active {transition: opacity 3s}
// 开始隐藏插入的元素的过渡状态过程
```



## 生命周期

### 生命周期的整个过程

1.在实例创建之前。（beforeCreate）

2.先创建一个Vue实例。

3.初始化成功，实例被创建（created）

4.查找是否有el属性，找到了就开始挂载和渲染，与dom元素构建联系。如果是组件则找到template。在挂载未完成的时候进入（beforeMount）

5.渲染完成以后进入（mounted）周期

6.修改了数据以后，会进入（beforeUpdate）和（updated）状态。即更新前和更新后的生命周期。会去检查虚拟dom。

7.渲染更新完成以后会有（beforeDestroy）销毁前和（destroyed）销毁后的生命周期。

### 使用ajax

一般ajax请求在created和beforeMount两个生命周期内。

## 自定义指令

### 自定义全局组件

```javascript
**main.js**
    
impotent ‘Back’ from ‘路径’    
Vue.component('Back',Back) //内容就是整个组件文件的内容
```

### 自定义指令

```javascript
**使用组件的vue**
<Back
 ：container="" //从父组件传值
></Back>
```

```javascript
**back.vue**
    <template>
    	<div
		  v-back-top="{container}"  //自定义指令。再绑定给这个指令。
		>
    	</div>
    </template>

	props：{
    	container //用来接收值。
	}
    directives:{
        'back-top':{  //自定义指令接收的地方。忽略v-
            inserted(el,binding){ //自带的钩子函数，可选。
                el:组件本身
                binding:一个对象。{
                name：指令名，不包括 v- 前缀。
value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。

oldValue：指令绑定的前一个值，仅在 update componentUpdated 钩子中可用。无论值是否改变都可用。

expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。

arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。

modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。
                }
                //在这里面定义事件
            }，
            bind（）{
                //第一次绑定到元素时调用，只调用一次。
            }，
            update（）{
                //组件更新时调用
            }，
            unbind：只调用一次，指令与元素解绑时调用，
            componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
        }
    }
```



## 扩展

### $event

```html
 <button @click="onNumAdd($event)">Add</button>

methods: {
        // 定义当前vue实例的方法
        onNumDecrease (e) {
          // 事件处理函数在v-on监听的时候不写括号,函数的第一个参数就是事件对象
          console.log(e)
        },
```

### ref

```html
<div ref="div">123</div>
this.$refs.div
// 获取该元素
```

### eventbus

**事件总线**

```htm
const bus = new Vue()  //创建一个空的vue实例

const ge = {
      template: '<div><span @click="onDawodi">打我弟</span></div>',
      methods: {
        onDawodi () {
          // 在点击的时候触发这个wuwuwu事件，di这个组件就能响应这个事件
          // 让bus触发一个自定义事件
          bus.$emit('wuwuwu', {
            kusheng: 'yinyinyin'
          })
        }
      }
    }
    
const di = {
      template: '<div><span>{{ku}}</span></div>',
      data () {
        return {
          ku: ''
        }
      },
      created () {
        // 一上来就要监听事件总线的wuwuwu事件
        bus.$on('wuwuwu', (data) => {
          // 响应ge组件里通过bus给我emit的这个wuwuwu事件
          // 接收到传递给我的参数data
          console.log(data)
          this.ku = data.kusheng
        })
      }
    }
    
    //两个不相干的组件的交互
```

### tbody

tbody里只能写tr，不能的组件

```html
<template id="my-tr">
    <tr></tr>
</template>

<tbody>
	<tr
    	is="my-tr"
        ></tr>
</tbody>
```

### mixins

#### 定义

混入一个对象的数组，可以像正常的实例对象一样包含选项。即可以拥有生命周期，filiters等等，都可以拥有。

```
//全局使用 **main.js**

Vue.mixin({
  filters: {
    countLt99 (value) {
      if (value > 99) {
        return '99+'
      } else {
        return value
      }
    },
    toFix2 (value) {
      return value.toFixed(2)
    }
  }
})
```





