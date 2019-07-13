## axios

### 1.安装

```html
npm i axios
yarn add axios
```

### 2.创建实例

```html
var ajax = axios.creat({
	baseUrl: "地址"，		//可配置多个
	timeout: "无响应时间"，  //可选
	headers: "自定义请求头"  //可选
})
```

### 3.请求

```html
ajax.get("路由")
	.then(resp =>{

})
```

### 4.真实项目中的用法

```html
Vue.prototype.$http = ajax
// 真实项目中，把该方法绑定在vue实例的原型中，这样每一个实例都可以使用这个方法了。

可以创建一个js文件来接收所有的请求，这样就不用把方法绑定到vue实例中了。
```

### 5.导出

```javascript
**请求文件**
import axios from 'axios'
const ajax = axios.create()

export const getHome = () => ajax.get('/api/tab/1?start=0')
```

```javascript
**vue组件**

import * as ajax from "@/request"
ajax.getHome().then
//固定写法，把所有的请求数据打包到ajax对象中。后面引入文件如果是index.js可以不用写。
```



### 6.拦截器

```html
ajax.interceptors.request.use(config => {
	
	return config //必填
})
在请求数据的过程中，还没请求到的时候。可以在return前执行一些时间。例如写一个等待图标。

```

```html
ajax.interceptors.response.use(resp => {
	
	return resp //请求到的数据
})
在请求数据的过程中，请求到的时候。可以在return前执行一些时间。例如让等待图标消失。

扩展：
ajax.interceptors.response.use(config => {
	if(resp.status === 200){
		return resp.data	
	}
	return config //必填
})
//如果请求的数据结构一样，都会返回一个相同的请求状态值和数据，则可以在拦截器里做判断，可以直接判断并只返回请求的数据。
```

### 7.this.nextTick

当ajax异步操作完成以后，比如渲染完data以后，再执行的函数，可以放进这里面执行。解决异步的问题。

### 8.请求头

#### referer

##### 作用

请求头的一部分，如果referer的内容是后端允许的地址，才被允许访问。可以用于防止别人盗链或者恶意请求，静态请求是.html，动态是.shtml。

```
axios.get(url,{
    headers:{
        referer:''
    }
})
```

