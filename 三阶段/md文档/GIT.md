## GIT

### git上传

git代码托管平台
	https://github.com/
	https://dev.tencent.com/
	https://gitee.com/

新建线上仓库
	new respository
	填写 respository name
	选择public
	得到一个线上地址

到本地代码的位置
	鼠标右键打开git bash here
	执行 git init
	配置用户名和邮箱
		git config --global user.name "ouyangzhouGIT"
		git config --global user.email "1248357097@qq.com"

	通过三步操作将本地代码推送到线上
		1、 git add .  
		 	将所有文件添加到缓存区
		2、 git commit -m "本次提交所做的修改"  
		 	将代码提交到本地仓库
		3、  git remote add origin https://github.com/XDLRanger/dary-project.git
			将本地仓库跟线上仓库进行关联
		4、  git push origin master
			将本地仓库推送到线上（可能需要多次输入用户名密码）
		5、 修改之后可以使用 git status查看状态
		6、 修改之后再执行
					git add .  
					git commit -m "本次提交所做的修改"  
					git push origin master
		7、 git clone git地址
		    把线上的仓库克隆到本地
### git分支

#### git branch

查看分支

#### git checkout -b 文件名

git checkout -b dev

新建一个dev分支并且切换到dev上，每个人写好代码以后，代码在dev里合并。

#### git checkout 文件名

切换分支

#### git branch -D 文件名

删除分支

### 代码新建合并请求

1.pull request

2.添加评审者

### 解决冲突

#### 变基

```html
git checkout dev //回到dev
```

```html
git pull origin dev //拉取老的dev查看区别
```

```html
git checkout 文件名 //回到新文件
```

```html
git rebase dev //列出两个分支间的区别
```

```html
git add . //添加到缓存区
```

```html
git rebase --continue //如果还有冲突就继续解决问直到出现
Applying： 文件名为止。
```

````html
git pull origin 文件名 //把改文件的旧版本拿下来更新以后再修改到dev里。
````

### git操作

#### git status

查看当前仓库状态

#### git diff 文件名 

查看修改的内容

#### git log 

查看最近提交的版本（commit后面的为版本号）

#### git reset --hard

add之后回退

#### git rm 文件名

删除文件

#### git mv 

移动

#### git checkout 文件名

当git add之前的项目需要回退的话

### 密钥

#### ssh-keygen

1.更改文件名，输入密码可以直接为空

2.输入以后在c盘的用户中有一个.ssh文件，里面就生成了公钥和私钥。

