# 设置账户
```bash
git config --global user.name "user name  "
git config --global user.email "user-mail@mail.com"
```
## 使用远程库
切记在账户ssh中添加ssh公钥，不然无法提交
```bash
ssh-keygen -t rsa -C "youremail@example.com"
```
此外在windows上貌似还有个小窗口会提示输入一次账号密码
## 版本控制系统
其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等,但doc等格式是二进制文件，无法跟踪，只知道大小变了

# 格式问题
千万不要使用Windows自带的记事本编辑任何文本文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符。

# 添加文件
```bash
git init   //初始化一个Git仓库。
```
# 添加文件到Git仓库，分两步：
```bash
git add <file>  //添加文件或者修改文件到暂存区  注意，可反复多次使用，添加多个文件；
git commit -m <message>   //(从暂存区)提交到分支并标注变更信息；
git status   //随时掌握工作区的状态
git diff    //查看变动的内容
```

# 查看日志
```bash
//显示从最近到最远的提交日志,加上--pretty=oneline参数显示简略信息
git log
//显示每次的命令及commit_id,再通过git reset --hard commit_id跳转,跳转时id不必写全，前几位即可
git reｆlog   
```

# 版本回退
```bash
//跳转版本，HEAD(当前版本)，HEAD^(上版本)，HEAD^^(上上版本)，HEAD~n(上ｎ版本)
git reset --hard commit_id  
```

# 撤销修改
## 仅工作区修改
```bash


```

## 仅提交到暂存区
```bash

```

## 提交到分支
```bash


```
# 删除文件
```bash
rm <file>    //本地删除文件
git rm <file>　　　//在git暂存库中删除
git commit -m "<messaage>"　　//将修改提交到分支
git checkout -- <file>    //未提交到暂存区之前通过该指令恢复内容
```

# 关联远程库
建立库之后按官网指示
```bash
//本地库
git init
git add README.md
git commit -m "first commit"
//连接远程库
git remote add origin git@github.com:username/reponame.git
//首次提交到远程库
git push -u origin master
//推送最新修改
git push origin master　　
//或者
git push
```

# 创建与合并分支
```bash
git branch		//1、查看分支
git branch <name>  		//2、创建分支
git checkout <name>  		//3、切换分支
git checkout -b <name> 		//4、创建+切换分支 (4=2+3) 
git merge <name>  	//5、合并某分支到当前分支, 合并分支时，如果可能，Git会用 <Fast forward> 模式，但这种模式下，删除分支后，会丢掉分支信息。当5中分支冲突时，会提供选项如何处理，处理完之后，再次提交
git branch -d <name>		//6、删除分支
git log --graph		//7、命令可以看到分支合并图。
```
#
