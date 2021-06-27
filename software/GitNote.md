# 版本控制系统
其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等,但doc等格式是二进制文件，无法跟踪，只知道大小变了

# GIT 操作

## 初始化

```bash
git init   # 初始化一个Git仓库。
```
## 工作区

![](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

工作区是我们存放文件的地方，工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

版本库是用来跟踪工作区文件，保存每次修改版本的地方。

保存到git库分两步：

1. 将工作区的文件手动添加到暂存区，方便后边一起提交；
2. 将暂存区的文件提交到git仓库中；

## 增加文件

### 添加文件到Git仓库

```bash
$ git add <file>  # 添加文件或者修改文件到暂存区  注意，可反复多次使用，添加多个文件；
$ git commit -m <message>   # (从暂存区)提交到分支并标注变更信息；
$ git status   # 随时掌握工作区的状态
$ git diff     # 查看变动的内容，未提交之前可用
```



```bash
$ echo 1111111 > 001.txt
$ git add 001.txt
$ git commit -m 'first'

$ echo 2222222 > 001.txt
$ git add 001.txt
$ git commit -m 'second'

$ echo 3333333 > 001.txt
$ git add 001.txt
$ git commit -m 'third'
```



## 修改提交

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

`git commit` 只会保存 `git add` 到暂存区的文件，如果修改之后没有保存到缓存区是无法提交到git库中的

提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

## 查看版本

```bash
# 显示从最近到最远的提交日志,加上--pretty=oneline参数显示简略信息
$ git log
commit c199a572408ace5d472275b0be5e3012e9e46de8 (HEAD -> master)
Author: tiaoteek <tuo*****@163.com>
Date:   Wed Jun 23 21:25:51 2021 +0800

    third

commit 5bf33ec14c28d71241a375031b3220dcdd904e79
Author: tiaoteek <tuo*****@163.com>
Date:   Wed Jun 23 21:25:28 2021 +0800

    second

commit 526d7014bcc4dd2ebe037f9862fac30031f412f0
Author: tiaoteek <tuo*****@163.com>
Date:   Wed Jun 23 21:24:37 2021 +0800

    first
```

```bash
$ git log --pretty=oneline
c199a572408ace5d472275b0be5e3012e9e46de8 (HEAD -> master) third
5bf33ec14c28d71241a375031b3220dcdd904e79 second
526d7014bcc4dd2ebe037f9862fac30031f412f0 first
```

**c199a572408ace5d472275b0be5e3012e9e46de8** (HEAD -> master) third

这串数字就是commit id，用来跳转版本用的

`git log`  **只能查看HEAD  之前的commit id**，如果跳转到以前的位置（HEAD从指向third到指向second），则**HEAD后边的commit id无法查看**(只能查看first,second,无法查看third的commit id)，这时需要`git reflog`

```bash
# 显示每次的命令及commit_id,再通过git reset --hard commit_id跳转,跳转时id不必写全，前几位即可
$ git reｆlog  
```



## 版本回退

```bash
# 跳转版本，HEAD(当前版本)，HEAD^(上版本)，HEAD^^(上上版本)，HEAD~n(上ｎ版本)
git reset --hard commit_id

# 实测
$ git log --pretty=oneline
c199a572408ace5d472275b0be5e3012e9e46de8 (HEAD -> master) third
5bf33ec14c28d71241a375031b3220dcdd904e79 second
526d7014bcc4dd2ebe037f9862fac30031f412f0 first

$ git reset --hard HEAD^
HEAD 现在位于 5bf33ec second

$ git log --pretty=oneline
5bf33ec14c28d71241a375031b3220dcdd904e79 (HEAD -> master) second
526d7014bcc4dd2ebe037f9862fac30031f412f0 first

$ git reflog
5bf33ec (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
c199a57 HEAD@{1}: commit: third
5bf33ec (HEAD -> master) HEAD@{2}: commit: second
526d701 HEAD@{3}: commit (initial): first

$ git reset --hard c199a57
HEAD 现在位于 c199a57 third
```



- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 撤销修改

### 仅工作区修改

如果用`git status`查看一下,文件还没`git add`到缓存区

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   001.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
$ git checkout -- <file>
```

```bash
$ git checkout -- 001.txt
```

### 仅提交到暂存区

用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   001.txt
```

用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

```bash
$ git reset HEAD 001.txt
Unstaged changes after reset:
M	001.txt
```

### 提交到分支

那就将库的版本回退

```bash
$ git reset --hard commit_id
```
## 删除文件
```bash
$ rm <file>    # 本地删除文件
$ git rm <file>　　　# 在git暂存库中删除
$ git commit -m "<messaage>"　　# 将修改提交到分支

$ git checkout -- <file>    # 未提交到暂存区之前通过该指令恢复内容
```

# 关联远程库

## 设置账户

```bash
git config --global user.name "user name  "
git config --global user.email "user-mail@mail.com"
```

传输人时候可以使用https方式，也可使用ssh方式，使用ssh可以仅开始设置一次，而**https每次提交到github都要输入密码**

## 使用ssh传输

看看有没有**.ssh**目录和这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有，可直接跳到下一步。否则创建SSH Key：

```bash
ssh-keygen -t rsa -C "youremail@example.com"
```

代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。

`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人

~~此外在windows上貌似还有个小窗口会提示输入一次账号密码~~

![](https://www.liaoxuefeng.com/files/attachments/919021379029408/0)



## 仓库绑定与解绑

### 仓库绑定

1、在本地创建文件夹，然后与远程文件夹绑定：

```bash
# 本地库
$ git init
$ git add README.md
$ git commit -m "first commit"
# 连接远程库
$ git remote add origin git@github.com:username/reponame.git
# 首次提交到远程库
$ git push -u origin master
# 推送最新修改
$ git push origin master　　
# 或者
$ git push
```

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；



2、在github创建库，通过clone的方式保存到本地：

```bash
$ git clone git@github.com:tiaoteek/test.git
正克隆到 'test'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
接收对象中: 100% (3/3), 完成.
```

### 仓库解绑

建议先用`git remote -v`查看远程库信息：

```bash
$ git remote -v
origin	git@github.com:tiaoteek/MyNote.git (fetch)
origin	git@github.com:tiaoteek/MyNote.git (push)
```

据名字删除，比如删除`origin`：

```bash
$ git remote rm origin
```

# 多分支

![](https://www.liaoxuefeng.com/files/attachments/919021987875136/0)



## 创建与合并分支(多人)

```bash
$ git branch		# 1、查看分支
$ git branch <name>  		# 2、创建分支
$ git checkout <name>  		# 3、切换分支
$ git checkout -b <name> 		# 4、创建+切换分支 (4=2+3) 
$ git merge <name>  	# 5、合并某分支到当前分支, 合并分支时，如果可能，Git会用 <Fast forward> 模式，但这种模式下，删除分支后，会丢掉分支信息。当5中分支冲突时，会提供选项如何处理，处理完之后，再次提交
$ git branch -d <name>		# 6、删除分支
$ git log --graph		# 7、命令可以看到分支合并图。
```




# 格式问题

千万不要使用Windows自带的记事本编辑任何文本文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符。