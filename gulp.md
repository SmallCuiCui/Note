#  gulp

自动化构建工具（fis3、gulp、grunt）
详细教程：http://www.ydcss.com/archives/18

* gulp是基于Nodejs的自动任务运行器， 她能自动化地完成 javascript /coffee/sass/less/html/image/css 等
* 文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后更新指定的这些步骤。

## 使用流程：

​    安装nodejs -> 全局安装gulp -> 项目安装gulp -> 项目安装gulp插件 -> 配置gulpfile.js -> 运行任务

### 1.安装 nodejs

node -v
npm -v

使用 npm 安装包：node package manager
npm install <package-name> -g （--save-dev）

-g 全局安装
--save 局部安装并保存到package.json配置中
-dev  存在package.json的devDependencies配置项里，意思是生产环境依赖的模块

文件：package.json(NodeJS项目的配置文件)


选择性安装 cnpm （http://npm.taobao.org/）:
npm install -g cnpm --registry=https://registry.npm.taobao.org
可以使用 cnpm 替代 npm 来安装资源



### 2.全局安装 gulp

命令：npm install gulp@3 -g		||	 cnpm install gulp@3 -g
测试：gulp -v

### 3.在项目目录下生成 package.json 文件：

命令：npm init		||	 cnpm init

* 在项目目录中本地安装 gulp,本地安装成功后，会生成 node_modules 文件夹：

pm i gulp@3	 或	cnpm install gulp --save-dev

### 4.在项目目录中本地安装 gulp 插件

* (搜索网站—>https://www.npmjs.com)：
* 安装 ：gulp-uglify，gulp-sass，gulp-htmlmin，gulp-connect

1. 压缩CSS命令：npm install gulp-minify-css --save-dev  	||	cnpm install gulp-minify-css --save-dev

2. SASS转CSS命令(编译SCSS)：npm i gulp-sass 	||	cnpm i gulp-sass

3. 压缩Html命令：npm i  gulp-htmlmin    ||    cnpm i  gulp-htmlmin

4. 压缩JS：

   ​	第一步：使用gulp-uglify压缩javascript文件，减小文件大小。 

   ​		命令：npm install --save-dev gulp-uglify   ||     cnpm install --save-dev gulp-uglify

   ​	第二步： 使用gulp-babelES6转ES5	
   	 	命令： npm install --save-dev gulp-babel @babel/core @babel/preset-env

   ​	  				||	npm install --save-dev gulp-babel@7 babel-core babel-preset-env

5. 新增本地服务器命令：npm install --save-dev gulp-connect

   

### 5.配置gulpfile.js 

*  src     :取源文件
* pipe    :管道（文件传输的过程)，可以在过程中对文件处理

* dest    :destination (目的地)管道中处理完后放到目标文件夹dest里
* watch：监听文件的的变化执行对应的任务
* ctrl+C 退出实时监听
* 给每一个任务加上pipe(connect.reload())

> ```
> const   gulp=require('gulp'),
>         uglify=require('gulp-uglify'),
>         minifyCss=require('gulp-minify-css'),
>         gulpSass=require('gulp-sass'),
>         htmlmin=require('gulp-htmlmin');
> //js
> const  babel=require('gulp-babel');
> //服务器
> const  connect=require('gulp-connect');
> //制定css任务   
>         /*
>             1.编译SCSS2.压缩CSS
>         */ 
> gulp.task('css',()=>{
>     gulp.src('src/css/*.scss')
>         .pipe(gulpSass())
>         .pipe(minifyCss())
>         .pipe(gulp.dest('dist/css'))
>         .pipe(connect.reload())
> 
> })
> //制定html任务
> gulp.task('html',()=>{
>     gulp.src('src/**/*.html')
>         .pipe(htmlmin({
>             removeComments: true,//清除HTML注释
>             collapseWhitespace: true,//压缩HTML
>             collapseBooleanAttributes: true,//省略布尔属性的值 <input checked="true"/> ==> <input checked />
>             removeEmptyAttributes: true,//删除所有空格作属性值 <input id="" /> ==> <input />
>             removeScriptTypeAttributes: false,//删除<script>的type="text/javascript"
>             removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
>             minifyJS:true,//压缩JS页面
>             minifyCSS:true//压缩CSS页面
>         }))
>         .pipe(gulp.dest('dist'))
>         .pipe(connect.reload())
> 
> })
> //制定js任务
> gulp.task('js',()=>{
>     /*
>        1.ES6转ES5,2.压缩js
>     */
>     gulp.src('src/js/**/*.js')
>         .pipe(babel({
>             presets: ['@babel/env']
>         }))
>         .pipe(uglify())
>         .pipe(gulp.dest('dist/js'))
>         .pipe(connect.reload())
> 
> })
> //制定libs任务
> gulp.task('libs',()=>{
>     //libs中是文件移动到dist里
>     gulp.src('src/libs/**/*')
>         .pipe(gulp.dest('dist/libs'));
> })
> //制定images任务
> gulp.task('images',()=>{
>     //libs中是文件移动到dist里
>     gulp.src('src/images/**/*')
>         .pipe(gulp.dest('dist/images'));
> })
> // gulp.watch :制定一个监听任务(耗费资源)
> gulp.task('watch',()=>{
>     //监听所有的html文件，一旦修改html,就执行
>     gulp.watch('src/**/*.html',['html']);
>     gulp.watch('src/js/**/*.js',['js']);
>     gulp.watch('src/css/**/*.scss',['css']);
> })
> //制定一个开启服务器的任务
> gulp.task('server',()=>{
>     connect.server({
>         root:"dist",
>         port:2222,//配置端口号
>         livereload:true//实时更新
>     });
> })
> //把任务集中执行 default必须有，是默认要执行的任务
> gulp.task('default',["html","css","js","libs","images","server","watch"]);
> 
> ```









> 文件合并
>
> cnpm install --save-dev gulp-concat
>
>
> gulpfile.js中修改js任务
> var concat = require("gulp-concat");
> gulp.task("js", function(){
>     gulp.src("src/js/**/*.js")
>  .pipe(babel({
>    presets: ['@babel/env']
>  }))
>     .pipe(concat('all.js'))
>     .pipe(uglify())
>     .pipe(gulp.dest("dist/js"))
>     .pipe(connect.reload());
> });
>







### 6.运行任务