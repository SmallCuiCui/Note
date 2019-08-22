# webpack

JavaScript程序的静态模块打包器，它会递归创建关系依赖图。。。

### 安装

#### 全局安装：

`cnpm install webpack -g`

`cnpm install webpack-cli -g`

#### 局部安装

新建项目文件夹(或直接在项目下)， -D模式局部安装上面两个插件。

### 打包

#### 无配置

默认打包： webpack ，按照默认的出口，入口，模式进行打包

指定模式打包：webpack --mode=development -W  也是无配置打包,-w的作用是当文件发生变化时，自动打包

#### 有配置

根目录下新建`webpack.config.js`文件，进行配置项的配置。注意名字不能改

### 核心概念
以下均是在`webpack.config.js`文件的配置。

#### 入口

绘制关系图的起点。默认入口文件： src/index.js

##### 单个入口的写法

直接是一个路径的字符串

~~~js
module.exports = {
  entry: './src/index.js'
};
~~~

##### 对象语法

~~~~js
module.exports = {
  entry: {
    app: './src/app.js',//应用程序
    vendors: './src/vendors.js'  // 第三方库
  }
};
~~~~

##### 多页面应用程序

webpack会构建多个独立的关系图

~~~js
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
~~~

#### 出口

配置最低要求，是一个对象，设置两个值：

* filename  输出文件的文件名
* path  输出文件的路径

入口文件可有多个，但是出口只有一个。默认出口文件： dist/main.js

~~~js
module.exports = {
  output: {
    filename: 'main.js',
    // filename: '[name].js', // 多个入口时的写法，对应上面最后输出为pageOne.js，pageTwo.js...
    path: path.resolve(__dirname,"dist")
  }
};
~~~

#### 模式mode

##### 用法

1. 配置项中添加mode字段。

   ```js
   module.exports = {
     mode: 'production'
   };
   ```

2. 打包时指定 webpack --mode=production

部分包只需要在开发的过程中使用，产品中不需要，比如webpack,eslint等，因此需要指定模式，再打包时可进行模式的选择。配置中设置mode字段

##### development

* 会将process.evn.CODE.ENV设置为development
* 没有压缩文件

##### production 

* 会将process.evn.CODE.ENV设置为production
* 会压缩文件

#### loader

解析或打包除了js以外的文件，都需要安装特定的loader。

##### 配置

~~~
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader','css-loader'] },
      { test: /\.(png|jpg|gif)$/, use: 'url-loader' }
    ]
  }
};
~~~

##### 常用loader

* css: 安装`style-loader`  `css-loader`进行config配置，配置时有顺序
* 编译sass：

* 图片文件：`url-loader`  `file-loader`  前置依赖后者，使用后者.
* 解析.vue文件：vue-loader,注意还需要用到一个插件VueLoaderPlugin
* 解析.ts文件：ts-loader

#### 插件

##### 自动引入文件

* `html-webpack-plugin`  -D局部安装。npm run build进行打包的原理。不需要自己引入文件，它以配置中指定的文件为模板，在dist下生成对应文件，自动为我们引入打包的文件。

  ~~~js
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  module.exports = {
  	plugins: [
          new HtmlWebpackPlugin({template: './index.html'})
        ]
  }
  ~~~

##### npm start原理

* `webpack-dev-server`   需要全局安装以及局部安装。

  开发过程中使用npm start的原理，打包命令使用webpack-dev-server 会将打包后的代码运行在localhost:8080下

  在项目下的package.json中配置script字段，使用npm start运行项目

  ~~~js
  // 配置webpack.config.js文件
  {
      mode:'development',
      devServer:{
          contentBase: 'dist/', 
          inline:true // 支持热更新
      }
  }
  ~~~

  ~~~js
  // 配置package.json文件下scripts字段
  "scripts":{
      // -w为热更新  --open为自动浏览器打开  --progress为显示进度
      "start":"webpack-dev-server -w --open --progress"
    }
  ~~~

#### babel：ES6转ES5

安装： `babel-loader`  `@babel/core`    `@babel/preset-env`

~~~js
rules: [
    {
        test: /\.js$/,
        exclude: /node_modules/,
        use:{
            loader: "babel-loader",
            options: {
                presets: ["@babel/preset-env","@babel/preset-react"]
            }
        }
    }
]
~~~

#### 搭建react环境

在上面操作的基础上进行，配置项就babel配置的preset添加预设即可。如上代码

安装  `cnpm install react react-dom @babel/preset-react`



国际化，删除。

react-loadable  按需加载





















































































