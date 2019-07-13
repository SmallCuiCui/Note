# webpack

><https://www.webpackjs.com/>
>
>指南：<https://www.webpackjs.com/guides/>

是一个模块打包器

## 1.使用

1. 安装

   ```bash
   npm install --save-dev webpack
   // 安装webpack
   
   $ npm i webpack webpack-cli -D
   # 等价于
   $ npm install webpack webpack-cli --save-dev
   // 安装webpack-cli
   ```

2. package.json

   ```json
   npm init -y //初始化package.json
   
   script: {
     "start": "webpack --mode development"
       //开发者环境
   }
   ```

3. 打包

   ```bash
   $ npm start
   # 默认 webpack 中入口文件在 './src/index.js' ，出口文件在 './dist/main.js'
   # 默认执行 webpack 命令是 production 模式，可以改为 development 模式
   ```

4. 多入口

   ```json
   module.exports = {
     entry: {
       index: './src/index.js',
       home: './src/home.js',
       cart: './src/cart.js',
     },
     output: {
       path: path.join(__dirname, "./dist"),
       filename: '[name].[chunkHash].js'
     }
   }
   ```

5.css

```js
**src/index.js**
    impor './index.css'
// 可以直接在index.js引入css，可以生效。
```



## 2.实际上线

### script

// 放置配置文件或者脚本文件的位置。

#### webpack.config.js

```js
**package.json**
    'scripts':{
        'dev':'webpack --mode development --config 
        scripts/webpack.config.js'
        //执行命令的时候找到webpack.config.js文件。
    }
```

```js
**webpack.config.js**
    output:{
        path:path.join(process.cwd(),'dist')或者
        path:path.join(__dirname,'../dist')
    }
//  改变path来控制出口文件放置的位置。
//  './'在非path的地方就是根目录位置
```

## 3.插件

### html-webpack-plugin

   ```bash
   $ npm i --save-dev html-webpack-plugin
   // 当js文件改变时动态生成html文件,并引入js
   ```
使用插件：

   ```js
**webpack.config.js**
    
const HtmlWebpackPlugin = require('html-webpack-plugin')
   
   module.exports = {
     .....,
     plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]                                      
}
   ```

### webpack-dev-server

自动生成一个服务器，用于打开项目

```js
npm install webpack-dev-server --save-dev //安装
```

```js
**package.json**
    'scripts':{
        'dev':'webpack-dev-server --mode development
        --config scripts/webpack.config.js'
        
        //npm run dev执行命令的时候找到webpack.config.js文件,并在开发环境打开服务器。默认8080端口。
    }
```

### css-loader

在webpack里处理css文件。

```js
npm install --save-dev css-loader style-loader

// style-loader在最后把生成的css打包成一个style标签放到渲染的结果head里。即内联样式表
```

```js
**webpack.config.js**
    
   module.exports = {
    rules: [
        {
            test: /\.css$/i,  
            //正则表达式。匹配css文件
            use:['style-loader','css-loader']
        }
    ]
                              
}
```

### mini-css-extract-plugin

生成css文件

```js
npm install -save-dev mini-css-extract-plugin
```

```js
**webpack.config.js**
   const MiniCssExtractPlugin =
   require('mini-css-extract-plugin')

   module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: '[name].[chunkHash].css'
        })
    ]，
    rules: [
        {
            test: /\.css$/i,  
            use:[MiniCssExtractPlugin.loader
       		，'css-loader']
            //style.loader不需要了
        }
    ]
                              
}
```

### sass-loader

sass转css

```
$ npm install sass-loader node-sass -D
```

```
**webpack.config.js**

   module.exports = {
    rules: [
        {
            test: /\.css$/i,  
            //正则表达式。匹配css文件
            use:[MiniCssExtractPlugin.loader
       		，'css-loader']
        },
        {
             test: /\.scss$/i,
              use:[
              MiniCssExtractPlugin.loader，
              'css-loader',
              'sass-loader' 
              //规律是从下往上执行
              ]
        }
    ]
```

```
**src/index.js**
    impor './index.scss'
```

### autoprefixer

自动兼容增加前缀

```
npm i postcss-loader autoprefixer -D //安装
```

```
**/postcss.config.js**

module.exports= {
    plugins:[
        require('autoprefixer')
    ]
}
```

```
**webpack.config.js**

   module.exports = {
    rules: [
        {
            test: /\.css$/i,  
            //正则表达式。匹配css文件
            use:[MiniCssExtractPlugin.loader
       		，'css-loader']
        },
        {
             test: /\.scss$/i,
              use:[
              MiniCssExtractPlugin.loader，
              'css-loader',
              'postcss-loader',
              'sass-loader' 
              //规律是从下往上执行
              ]
        }
    ]
```

### file-loader/url-loader

处理图像资源

### babel-loader

```
npm i -D babel-loader @babel/core @babel/preset-env
```

ES6转ES5

```
**webpack.config.js**

   module.exports = {
    rules: [
		{
            test:/\.m?js$/,
            exclude:/(node_modules|bower_components)/,
            use:{
                loader:'babel-loader'.
                options:{
                    presets:['@babel/preset-env']
                }
            }
		}        
    ]
}
```

