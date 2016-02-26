<h1>NoteEx</h1>

***


<h4>contents</h4>

* [CocoaPods](#cocoapods)
* [Git代码管理](#git)
* [崩溃日志分析](#crash)
* [AutoLayout](#AutoLayout)
* [JSPatch](#JSPatch)
* [SQL && FMDB](#SQL && FMDB)

<h2 id="cocoapods">CocoaPods</h2>

**替换ruby软件源，升级gem**

	gem sources --remove https://rubygems.org/
	gem sources -a http://ruby.taobao.org/
	gem sources -l
**使用gem安装pod**

	sudo gem install cocoapods
	pod setup  （.cocoapods文件）
	from now 可以使用 pod 命令了
**创建podfile**

	pod init
	需要添加的库就放在这个文件
	
**查找第三方库**
	
	pod search <name>
**下载三方库&设置编译参数**

	cd 到工程
	pod install
	
	每次更改了Podfile文件，你需要重新执行一次
	pod update命令
	
	每次install 或者 upate时候，默认会先更新podspec。浪费时间好么~~在后面加上--no-repo-update  可以禁止对podspec的更新操作
	
**使用私有库**
	
	以后再说~
	
	
---

<h4>使用体验</h4>

1.install之后  好多红色的文件  表示怕怕的~~
*别看文件都是红色儿的  还不能删 ··*
*stack上提供给了好多好多解决方案，改scheme啊、更改other link flags*



2.三方库下载到工程之后，import时候没有提示  很蛋疼！！

***target-build setting-user Header search path-添加$(SCROOT) 并且选中recursive  【有时候添加$(PODS_ROOT)】*** *问题解决*


3.没有使用pods之前  项目已经导入的那些库 怎么搞~

4.update/install时候  等的时间好长好长。。。



<h2 id="git">git代码管理</h2>

*Xcode的commendtool已经自带了git  就不用安装git了*

*还有其他几种git安装方案 [git安装](http://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)*

###1.config
·git config --global user.name "name"

·git config --global user.email "email"


**面向git全局的 用户信息 所以  也不用去仓库就能进行的命令**


###2.repository
**mkdir <file folder> -> git init -> git  add someFile -> git commit (-m "explain"")**(后续命令 都是在当前的git仓库中进行的)

至此 完成了git仓库的初始化

###3.git 命令
`git status` 当前仓库的状态

`git diff` 查看修改的地方  eg:git diff readme.md

每次修改文件之后  先add，然后commit  可以通过上面两个命令来查看当前git的状态

`git log` 查看commit的历史记录 (--pretty=oneline 每次的变更 在一行显示  看起来跟家直观)

	·[git reset --hard HEAD^]  
		表示回到上一版本 (同理：回到上上版本HEAD^^ ..... HEAD~100)
		
	·[git reset --hard 1ec156dada9](取前面几位就行)
	
	--> 有时候，想暴力回归上一版本，可以使用这一命令。（还有其他方法也能实现这一效果）
	
	只要知道commit的版本号 （就是git log时候那一大长串） 就能reset到指定的版本。eg:  
	·[git reflog] 记录每一次命令 

		
###4.working space &&  repository

**概念：**
工作区：可见的文件夹就是一个工作区；

版本库：隐藏目录.git。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

git add把文件添加进去，实际上就是把文件修改添加到`暂存区`；

	git add f1 f2 f3...
	git add dir  //添加改文件加下所有子文件
	git add . //添加当前目录下的所有目录和子文件

git commit提交更改，实际上就是把`暂存区`的所有内容提交到`当前分支`。

	

git diff HEAD -- readme.md  可已查看工作区和版本库里面最新版本的区别

git checkout -- readme.md 就是让这个文件回到最近一次git commit或git add时的状态。

###5.重点来了--远程仓库

github 其实只是一个提供Git仓库托管服务的网站。只要注册一个GitHub账号，就可以免费获得Git远程仓库

本地Git仓库和GitHub仓库之间的传输是通过SSH加密的。根目录下有一个.ssh的隐形文件夹。里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对

SSH KEY 的作用：因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

1）添加ssh key。id_rsa.pub VC到账户
2)新建远程仓库。github 提示有三种关联方案
(本地已有的仓库：
git remote add origin https://github.com/iTiny/NewRepo.git
git push -u origin master)

***至此 本地仓库的内容已经同步到了远程仓库**
以后 把本地库推送到远程库上，使用git push 命令了
 `git push origin master`
 
 
###6.远程仓库的克隆

`git clone`命令

git clone https://github.com/iTiny/NewRepo.git

*最好使用ssh来克隆*


###7.分支bransh

分支的意义：创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

1）*创建且切换到该分支上* 

git checkout -b branchName 

相当于 git branch branchName  -> git checkout branchName

2)*查看当前分支*
 git branch branchName

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>


3)*默认的git merge 使用的fast forward模式，不保留分支信息，（删除分支后）,git merge --no-ff -m "description" branchName* (相当于禁用了fast模式，多了commit，，这样，在git log 中就能查看commit信息了)

###8.git stash

如果在某分支上进行正常的开发，急需要修复项目中的bug。怎么办？？？工作只做了一半，没办法提交，bug有急得一B。。。git stash 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。

git stash命令之后，git status 是干净的

然后可以放心大胆得checkout新的bug 分支，修复bug 提交 合并 ，回到原来的工作分支  继续干活！！

怎么回？？？--> 
	·git stash list
	·method1:  git stash apply (stash内容不删除，需要git stash drop来删除)
	·method2: git stash pop (同时吧stash内容页删除了)
	

----
删除没有合并完全的分支，如果直接使用git branch -d   删不了

需要强制删除 git branch -D branchName



###9.多人协作

1.查看远程分支  `git remote` (默认是origin)

2.克隆&push前面说过

但是，直接克隆，只能看见master分支  如何在origin的dev分支上继续呢 
`$ git checkout -b dev origin/dev`

3.git tag <name> 给分支打标签（前提是切换到需要打标签的分支上）

`git tag`查看标签

`git tag -d v2`删除tag

`git push origin v2.2` || `git push origin --tags`


===
*clone repo需要输入username & pasw  都是github的账号密码*

*查看远程仓库的名字（默认是origin） `git remote -v`*

*master是分支名  分支的boss，，而origin是远程仓库默认的命名。。git add, git commit，本地的master都会跟着移动，merge之后，远程的master分支一会相应改变*

**


1.克隆远程仓库

git clone URL  ||  github 客户端   进行克隆

2.查看远仓库git remote (-v可以查看克隆地址)


---
**一直纠结 怎么删除github远程的仓库。。⊙﹏⊙b汗。在该仓库的setting里面（右侧）有个醒目的danger zone,下面有删除仓库的操作**



	
<h2 id="crash">崩溃日志分析（.crash&&.csv）</h2>


###通过.crash文件
*.crah是应用使用过程中，由于bug或者用户不友好操作造成程序非正常退出时产生的崩溃日志文件。日志文件是分析错误发生的重要依据，但是原生的日志文件不是很直观。获取到具体是哪一行代码出现问题，需要一系列的操作才行！*

1.获取.crash：
	
	1）iTunes同步之后，崩溃日志信息在~/Library/Logs/CrashReporters/MobileDevice 目录下面(设备的crash记录)
	2）通过Xcode，device下的view device logs查看.crash (export出来就是了)
	

###通过.csv
.csv文件是友盟错误统计导出后生成的文件，需要借助友盟图形化工具umcrashtool，来需找错误具体出在哪一行。
`终端->umcrashtool(拖入)->.csv(拖入)->enter` ==>同一文件夹中生成了-symbol.csv文件，里面有问题代码具体是哪一行的信息~~
[umcrashtool使用方法](http://dev.umeng.com/analytics/reports/errors#2)


<h2 id="Sublime">Sublime</h2>

[安装Sublime Text 3插件的方法](http://www.cnblogs.com/Rising/p/3741116.html)

cmd+shift -> 调出命令窗口 -> input install  ->  会出现插件列表  -> 安装想要的插件


	
<h2 id="AutoLayout">AutoLayout</h2>

<h2 id="JSPatch">JSPatch</h2>

[API功能介绍](https://github.com/bang590/JSPatch/wiki/Usage-of-defineClass)

1 取代原生OC的方法：
		
		"_" => 方法命中含有多个参数，用_链接
		如果方法命中含有_ , 则使用 __ 来代替
		"__func" => -(void)_func{} :用"__"取代特定_func特定格式方法
		"ORIG" => 调用原始方法 eg. self.ORIGfunc();  
			
		调用父类的方法：self.super() eg.self.super().viewDidLoad();
		属性的get/set:  eg. var data = self.data()
							self.setData(data.toJS().push("JSPatch"))
		

2.传递Block

		一般：
		var func = function(b){};
		var blk = block("BOOL",func)
		func(12)
		=================================
		block里面包含block参数:
		[ block参数要写成NSBlock * ]
			var blk=block("BOOL,NSBlock *",function(b,blk)){
				if(b){
					blk(true)
				}
			}
		
		=================================

		在 Objective-C 中传入到 JSPatch 中的 Block 会转换为 function，如果需要再将该 Block 传回到 OC，依旧需要用 block(paramTypes, function) 封装。
		//Objective-C代码
	- (void)excuteBlock:(JSBlock)block {
	    if (block) {
	        block(@"Hello, JSPatch");
	    }
	}
	- (JSBlock)returnBlock {
	    JSBlock block = ^(NSString *str) {
	        NSLog(@"%@", str);
	    };
	    return block;
	}
	
	//JSPatch脚本
	var blk = self.returnBlock()
	self.excuteBlock(block("NSString *", blk))
		
		=================================
		
	JSPatch中使用block中使用self，需要将self赋值给一个变量，然后再使用
	var slf = self
	self.requestWithCompletion(block("BOOL, NSError*", function(success, error){
	    slf.label().setText("complete")
	})
	)
	
3.JSPatch 中的 NSArray，NSString，NSDictionary

	//Objective-C
	- (NSArray *)returnNSArray {
	    return @[@"Objective-C", @"Swift"];
	}
	
	var nsarray = self.returnNSArray()
	var array = ["Objective-C", "Swift"]
	//这两者不能混用，因为他们是不同的类型。
	
	var wrongStr = nsarray[0] //得到的是一个不正确的对象，请不要使用取下标的方式获取NSArray/NSDictionar对象。
	
	var rightStr = nsarray.toJS()[0] //先unbox返回的对象，然后取出其中的数据
	或
	var rightStr = nsarray.objectAtIndex(0) //使用Objective-C方法获取NSArray/NSDictionary中的对象
	
	
<h2 id="SQL && FMDB">SQL && FMDB</h2>

 
 ---
* SELECT
* UPDATE
* DELETE  
* INSERT INTO
* CREATE DATABASE
* ALERT DATABASE
* CREATE TABLE
* ALERT TABLE
* DROP TABLE
* CREATE INDEX
* DROP INDEX

----

SELECT * FROM tableName
