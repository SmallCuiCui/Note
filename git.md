### 常用命令行：
##### 新建文件夹

​	mkdir filename  //新建文件夹

##### 查看文件夹下内容

​	ls  //查看当前文件夹下的内容列表

##### 查看文件内容

​	cat readme.txt   //查看文件内容

### git学习

##### git本地配置git账户

git本地安装之后配置自己的账户
​	git config --global user.name "..."
​	git configr --global user.email "..."
​	-global即为全局，表示本台机器所有仓库都使用这个配置，但也可以为某个仓库使用不同的用户名及邮箱

##### 本地创建仓库
​	先cd到一个放库文件夹的位置，新建库文件夹(mkdir filename)，然后cd到库文件夹，再初始化仓库(git init)
​	mkdir filename  命令行新建文件
​	git init  初始化本地版本库

##### 把文件添加到仓库
​	先cd到库文件夹下，即仓库.git所在文件夹下，同时要添加的文件也必须在该文件夹下。两步，先添加(add)，再提交(commit)
​	git add readme.txt  //即将readme.txt文件添加到当前仓库，可多次添加，然后再一次性提交
​	git commit -m "message"  //告诉git将文件提交到仓库，

##### -m注释内容标注

* feat：新功能（feature）

* fix：修补bug

* docs：文档（documentation）

* style： 格式（不影响代码运行的变动）

* refactor：重构（即不是新增功能，也不是修改bug的代码变动）

* test：增加测试

* chore：构建过程或辅助工具的变动

  例：git commit -m "fix:bug1"

##### 掌握当前仓库状态：
​	git status  //可以随时监控到仓库提交文件与本地文件状态，知道本地是否修改过，是否添加，提交之类状态

##### 掌握文件修改内容
​	git diff   //详细告诉被修改过的文件的修改内容，包括修改前与后的对比

##### 查看提交历史记录 
​	当前版本到最远的一次版本，不包括回退过的版本，即不包括HEAD后的版本
​	git log  //显示最近到最远的提交日志记录，包括版本号，作者，时间，描述
​	git log --pretty=oneline  //也是查看记录，但是信息输出更简洁，包括版本号和描述
​	了解：commit参数后面的一长串字符串是指版本号，每一次提交一个版本号，方便用于查看，回退到指定版本

##### 版本回退

本地文件夹内容会跟着版本改变
​	HEAD(当前版本) ,HEAD^(上一个版本),HEAD^^(上上个版本)  *我的电脑上没效果
​	git reset --hard HEAD^   //回退到上一个版本
​	git reset --hard 12d5d  //回退到指定版本，12d5d为版本号的前几位，只需要前几位即可。可倒退或回到任意版本，只要知道版本号即可

##### 查看git命令历史
​	git reflog  //即使电脑关机重启也是存在的，可用于查看回退过的版本号记录。

##### 删除本地仓库

：即删掉.git文件即可
​	命令行方式：rm -rf .git（需要cd到该库文件夹下）

##### git概念
​	工作区		版本库(暂存区+分支)	暂存区		     分支					HEAD(指向分支的指针)
​	learngit		.git			index(.git下的文件)	     创建版本库时会创建一个唯一主分支master(.git下)

Git跟踪并管理的是修改，而非文件。每次修改，如果不用git add到暂存区，那就不会提交
​	git add 就是把文件修改添加到暂存区，可添加多次
​	git commit  把暂存区的内容提交到当前分支，一次性提交多次添加的文件。
​	一旦提交了，工作区就是干净的，使用git status可查看状态

撤销工作区的修改内容
​	手动修改本地文件
​	或使用git：git chechout -- readme.txt  //撤销redme.txt的在工作区的修改。注意命令中两个--且两边都有空格
​		两种情况，一是上次commit后仅在工作区修改，使用checkout会直接撤销修改。
​			二是add后在工作区做了修改，使用checkout会回到add的版本
​			总之就是撤销最近一次仅在工作区的修改。checkout不能撤销add过的内容
撤销暂存区的修改内容，分两步
​	git reset HEAD readme.txt  //撤销上次暂存区的readme.txt修改到工作区
​	git checkout -- readme.txt  //在使用checkout撤销在工作区的修改
撤销提交的修改内容  -即版本回退
​	git reset --hard 12d5d

删除工作区的文件  两步
​	单纯把文件夹下的文件删除时，git会察觉到工作区和版本库不一致
​	git rm test.txt  //删除已经提交到分支上的test.txt文件
​	git commit -m "remove test.txt"  //删除后需要提交
#####  恢复工作区删除的文件
​	git checkout -- test.txt   //恢复刚从工作区删除的test.txt文件

关联github上的远程库
​	git remote add origin git@github.com:SmallCuiCui/learngit.git   //git@github.com:SmallCuiCui/learngit.git是自己git账户上面的一个仓库的ssh

把当前本地仓库的分支推送到远程库
​	git push -u origin master  //第一次把本地仓库分支上的所有内容推送到远程库
​		第一次使用时加了-u参数，不仅把本地分支内容推送到远程分支，还会把本地分支与远程分支关联起来，之后推送就可以简化命令
​	git push origin master  //推送本地master分支的内容到远程

#####  从远程库克隆到本地
​	git clone git@github.com:SmallCuiCui/gitskills.git   //将远程的gitskills仓库克隆到本地

##### 分支相关

：每次提交Git将它们串成时间线，这条时间线就是一条分支。Git中有唯一条主分支master和其他若干新创建的分支。HEAD指向当前分支
一开始只有master分支，master指向最新提交，再用HEAD指向master，就能确定当前分支了。
当新建一个叫dev的分支时，将HEAD指向dev，表示当前分支在dev，随后的修改提交都在dev分支上，而master主分支不变
当在dev上完成工作后就合并dev分支与master分支，然后还可以删除dev分支

* 创建分支+切换分支
  ​	git checkout -b dev   //创建dev新分支，-b表示切换到新创建的dev分支
  ​		相当于先创建：git branch dev(此命令仅创建分支，未切换)，然后切换：git checkout dev

* 创建新分支
  ​	git branch dev  //创建dev分支，此命令仅创建分支，未切换

* 切换分支
  ​	git checkout dev  //将当前分支切换到dev分支，即将HEAD指针指向dev

* 显示本地分支：
  ​	git branch   //列出本仓库的所有分支，当前分支前会有*号

* 合并指定分支到当前分支

  ​	git merge dev  //把dev分支与当前分支合并，即当前分支与dev的内容相同，以dev分支的最新提交完全一样。其他分支不变
  ​	如果当前分支是master，则合并就是将master指针指向dev所指向的地方，也就是dev的最新提交
  ​	此类情况是一个超前提交，一个相对滞后，进行快速合并(fast forward)这种合并看不出来曾今做过合并

* 删除分支

​	git branch -d dev  //删除dev分支，删除分支时不能删除当前所在分支。此分支的修改必须合并后才能删除
​	git branch -D dev   //强制删除分支，此分支可以未进行合并

* 查看分支历史

​	git log --graph
​	git log --graph --pretty=oneline
​	git log --graph --pretty=oneline --abbrev-commit  //最简洁

##### 解决冲突

​	：当合并的两个分支都有新的提交，但是内容却不一致，此时会发生冲突而不能合并

​	此时合并会提示有冲突无法合并，使用git status可以知道，直接cat查看文件内容可以直接知道不同点
​	解决办法：修改其中一个分支的文件与另一分支一致，然后add,commit,这样就可以合并了

##### 分支管理策略
​	快速合并方式(Fast forward)方式，在删除分支之后会丢掉分支信息，分支历史上就看不出合并
​	git merge --no-ff -m "merge with no-ff" dev   //强制禁用快模式(--no-ff)的合并方式，
​		强制禁用Fast forward模式，Git就会在merge时生成一个新的commit来记录分支信息，所以需要-m添加一个描述
​	实际开发时，master仅用于发布新版本，是稳定的。dev用于干活的，每个人都在上面合并，每个人也都有自己的分支。

##### Bug分支的管理
​	(当前分支存在未提交的修改)储存现场，切换到bug分支，新建一个分支来处理bug，合并到处理完bug的分支(到此已修复bug)，回到原工作分支，恢复现场继续工作
​	git stash  //将当前工作现场(dev分支)储存，使用git status可以看到一个干净的工作区
​	git checkout master  //切换到bug分支，假设为master分支
​	git checkout -b issue-101  //新建分支来修复bug
​	git add reademe.txt  //添加修复的bug
​	git commit -m "fix bug101"  //提交修改
​	git checkout master  //切换回master
​	git merge --no-ff -m "merged bug fix 101" issue-101   //禁用快合并方式来合并分支
​	git branch -D issue-101  //删除修复bug的分支
​	git checkout dev   //回到自己工作的分支继续工作
​	(可用git status查看当前为干净的环境，使用git stash list可查看保存的工作现场stash内容)
​	git stash pop  //恢复现场并删除stash内容，相当于先git stash apply恢复，然后git stash drop删除stash内容

##### 多人协作下的推送
​	不同人之间的提交存在冲突时，git push dev不能成功推送，先用git pull把最新提交抓取下来，在本地合并，解决冲突，再推送
​	同事A：从远程clone仓库到本地，默认只有master分支。
​		git checkout -b dev  //新建分支工作，工作完毕后添加，提交
​		git push origin dev  //将dev分支push到远程(此时远程有两个分支dev/master)
​	自己：也在dev分支上添加，提交了相同文件，push失败，说明与最新提交有冲突，所以要先把最新提交抓取(git pull)下来，解决冲突再push
​		git pull失败的话(no tracking information)：使用git branch --set-upstream-to=origin/dev dev   //先将本地dev与远程dev分支进行链接
​		git pull  //相当于把远程的最新提交与本地试图合并,存在冲突，手动解决冲突，然后再推送
​		git push origin dev  //解决冲突之后再推送
​	
​		
​	