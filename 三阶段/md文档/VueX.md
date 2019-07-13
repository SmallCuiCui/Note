## VueX

### 原理

state数据源渲染vue，视图view修改先通过发送一个actions，actions提交一个改变mutations，再改变事件源。所有vuex是一个单向的数据改变。

vuex是单例模式。

### 安装

npm i vuex -S

### 用法

```javascript
vue实例中的$store中可以看见。


export default new Vuex.Store({
    state: {
        //放置元数据
    },
    getters:{
        //return关于元数据的一些信息，例如length
    },
    actions:{
        //异步函数，例如settimeout里执行的函数还是在mutations里执行。
    },
    mutations:{
        //控制元数据的公用方法。
    },
    modules:{
        
    }
})
```

### 实际项目

#### 创建store文件夹

#### 创建文件

##### index.js

```javascript
// 总管理文件
import Vue from 'vue'
import Vue from 'vuex' //引入两个插件
import state from './state.js' //引入元数据文件

Vue.use（Vuex） //使用中间件

export default new Vuex.Store({ //创建一个实例
    state 
})
```

##### state.js

```javascript
export default{
    
}
```

##### mutations.js

```javascript
export default {
    fn(state){
        
    }
}
```



##### actions.js

##### getters.js



#### main.js

```javascript
import store from '@/store/index.js'  //全局引入
```

#### 组件中

```javascript
import {
	mapState
    mapMutations
} from 'vuex'  //在组件中

computed:{
    ...mapState(['元数据名'])
    ...mapGetters(['自定义名'])
}

methods:{
    ...mapMutations(['自定义名'])
}
```

### 开发环境

```javascript
**store里的index.js**
//如果项目不上线，就用严格模式。否则就不用，以免报错。
    export default new Vuex.Store({
		strict: process.env.NODE_ENV=== 'development'
		//为true就是开发环境，即开启严格模式。
})
```






