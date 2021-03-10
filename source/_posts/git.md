---
title: Git总结 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: #文章分类目录，可以为空，注意:后面有个空格
tags: [Git] #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
description: git#概要

---


# git branch

```bash
git branch ## 查看当前所在分支
```

## 新建分支

```bash
git branch aaa
```

建议使用[git switch](#git switch)

## 本地分支与远程关联

```bash
git branch --set-upstream-to=origin/<branch Name> <branch Name>
```

## 取消与远程关联

```bash
git branch --unset-upstream
```

取消与远程仓库的关联`git remote remove origin`

## 删除本地分支

```bash
git branch -d <branch name>
```

# git checkout

## 分支操作

```bash
git checkout aaa # 切换到 aaa分支
git checkout -b aaa # 创建aaa，然后切换到 aaa分支
git checkout commitid # 切换到某个commit id
```

## 丢弃本地所有修改，不包括新增文件

```bash
git checkout . //本地所有修改的。没有的提交的，都返回到原来的状态
```

# git switch

```bash
git switch aaa # 切换到 aaa分支
git switch -c aaa # 创建aaa，然后切换到 aaa分支
```

| 管理分支        | git branch      | git branch    |
| --------------- | --------------- | ------------- |
| 切换分支        | git checkout    | git switch    |
| 新建+切换分支   | git checkout -b | git switch -c |
| 切换到commit id | git checkout    | git checkout  |

# git restore

```bash
git restore [--worktree] aaa # 从staged中恢复aaa到worktree
git restore --staged aaa # 从repo中恢复aaa到staged
git restore --staged --worktree aaa # 从repo中恢复aaa到staged和worktree
git restore --source dev aaa # 从指定commit中恢复aaa到worktree
```

<img src="https://i.loli.net/2020/12/30/APhoDGuC1TQw2Oy.png" alt="542.png" style="zoom:67%;" />

# git stash 

可以将本地还没有提交的改动全部存储起来 并不提交
## git stash apply / git stash pop
这两个命令就可以将刚才暂存起来的内容还原了。但是这里有一个问题，就是 stash apply 和 pop 之间是不同的。
这里涉及到 stash 内部的实现机制，stash 内部其实是通过堆栈实现的。pop 对于堆栈而言很明确，就是弹出的意思。也就是说如果我们使用的是 pop，那么当我们 pop 之后，这条记录会在堆栈当中删除。而如果使用的是 apply 呢，记录不会从堆栈当中删除，仍然会保留下来。
一般情况下我使用 pop 多一些，但是 pop 也有缺点，比如 pop 没有办法选择应用的记录。我们可以使用 git stash list 来查看一下当前堆栈当中已经有的记录。
如果我们使用 git stash pop 的话，默认的是应用的栈顶的记录，也就是 stash@{0}。但如果我们使用 stash apply 的话，我们可以自由选择我们想要应用的记录。比如如果我们想要应用最后一条记录的话，我们可以这样：

```bash
git stash apply stash@{2}
```
清空`git stash clear` 删除某个`git stash drop stash@{0}  `

脚注示范[^1]

# git diff

查看当前文件的改动信息

```bash
git diff <filename>
```

查看改动文件

```bash
git diff --stat 
```

查看与另一分支的差别

```bash
 git diff <branch> <filename>
```

查看与暂存仓库的差别

```bash
git diff --cached <commit> <filename>
```

查看与远程仓库的差别

```bash
git diff <commit> <filename>
```

`<cmmit>`=`HEAD`时查看工作目录同最近一次 commit 的内容的差异。

两次commit之间的差别

```bash
git diff <commit> <commit>
```

以上命令可以不指定 `<filename>`，则对全部文件操作。
 以上命令涉及和 Git仓库 对比的，均可指定 commit 的版本。

- `HEAD` 最近一次 commit
- `HEAD^`  上次提交
- `HEAD~100` 上100次提交
- 每次提交产生的哈希值
# git add
git add .

不加参数默认为将修改操作的文件和未跟踪新添加的文件添加到git系统的暂存区，注意不包括删除

git add -u

-u == --update 表示将已跟踪文件中的修改和删除的文件添加到暂存区，不包括新增加的文件，注意这些被删除的文件被加入到暂存区再被提交并推送到服务器的版本库之后这个文件就会从git系统中消失了

git add -A

-A == --all 

表示将所有的已跟踪的文件的修改与删除和新增的未跟踪的文件都添加到暂存区。

git add -I 不常用

## 撤销add

如果是某个文件回滚到上一次操作：  
```
git reset HEAD  文件名
```
**撤销** **add****操作**

可以直接使用命令  **git reset HEAD**

这个是整体回到上次一次操作

**绿字变红字** **(撤销add)**

如果是某个文件回滚到上一次操作： **git reset HEAD** **文件名**

## 批量添加 （不是add .）

在工作中，使用add . 的情况反而不是最多的，工作区有很多文件不能提交到仓库里，一般情况下需要我们一个一个添加，你的目的是要添加所有的还是需要一种方式添加你需要添加的，如果是前一种 `git add .` 就可以

如果这里还有特殊情况 比如一半需要添加一半不需要 使用 `git add -i` 进入交互式添加

<img src="https://i.loli.net/2020/12/30/qAfQ2WjIslnpaOK.png" alt="image-20201217171840684.png" style="zoom:67%;" />

<img src="https://i.loli.net/2020/12/30/GXfs6C7Si9oTvdh.png" alt="image-20201217171922907.png" style="zoom:67%;" />

<img src="https://i.loli.net/2020/12/30/Jfi4WSl5VQmt9XY.png" alt="image-20201217171948829.png" style="zoom:67%;" />

<img src="https://i.loli.net/2020/12/30/KvG8Bc3lr2nhUmZ.png" alt="image-20201217172008565.png" style="zoom:67%;" />

# git pull

默认拉取远程已绑定的分支到当前分支

```bash
git pull
```

```bash
//与这两句等价
git fetch origin master //从远程主机的master分支拉取最新内容 
git merge FETCH_HEAD    //将拉取下来的最新内容合并到当前所在的分支中
```

取回`origin`主机的`fixbug`分支的最新提交，**并与本地的`master`分支合并**

```bash
git pull origin <remotebranshname>:<localbranshname>
```

指定远程分支与当前分支合并

```bash
git pull origin <remotebranshname>
```

<img src="https://i.loli.net/2020/12/30/MrpnYvLAG6jzXiW.jpg" alt="clip_image001.jpg" style="zoom:50%;" />

详情参考[git pull/fetch详解](# git fetch & pull详解)

# git push

当前分支只有一个远程分支 可用此命令

```bash
git push origin master
```

如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建

```bash
git push origin : refs/for/master 
```

如果省略本地分支名，则**表示删除指定的远程分支**，因为这等同于推送一个空的本地分支到远程分支，等同于 git push origin --delete master

git push origin

如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支

 git push的一般形式为 git push <远程主机名> <本地分支名>  <远程分支名> ，例如 git push origin master：refs/for/master ，即是将本地的master分支推送到远程主机origin上的对应master分支， origin 是远程主机名，第一个master是本地分支名，第二个master是远程分支名。
## 撤销push操作
重置至指定版本的提交，达到撤销提交的目的
```
git reset –-soft <版本号>
```
# git commit 

## 使用amend命令修改commit信息

(注： amend命令只会修改最后一次commit的信息，之前的commit需要使用rebase）

```bash
git commit --amend --reset-author
```
## 撤销commit
```
git reset --soft HEAD~1  //windows的bash
```
## 查看提交历史记录
*git log*

查看所有的commit提交记录

加上参数  --pretty=oneline，只会显示版本号和提交时的备注信息

*git reflog*

 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

*git show*

 查看提交的详情

- 1.查看最新的commit

*git show*

- 2.查看指定commit hashID的所有修改：

*git show < commitId >*

- 3.查看某次commit中具体某个文件的修改：

*git show < commitId fileName >*

## Commit message 的格式
### 传统方式
每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。Header 是必需的，Body 和 Footer 可以省略。

（1）type

type用于说明 commit 的类别，只允许使用下面7个标识。
```
feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动
```
（2）scope

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

（3）subject

subject是 commit 目的的简短描述，不超过50个字符。
```
以动词开头，使用第一人称现在时，比如change，而不是changed或changes
第一个字母小写
结尾不加句号（.）
```
### blingbling emoji
To use gitmojis from your command line install gitmoji-cli. A gitmoji interactive client for using emojis on commit messages.
```
npm i -g gitmoji-cli
```

*常用[emoji](https://gitmoji.carloscuesta.me/)*

:memo: :pencil:Add or update documentation.
:pencil2:Fix typos.
:bug:Fix a bug.
:rocket:Deploy stuff.
:sparkles:Introduce new features.
:tada:Begin a project.
参考：
[Git 写出好的 commit message](https://ruby-china.org/topics/15737)
[Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.htm)
[git commit 时使用 Emoji ](https://zhuanlan.zhihu.com/p/29764863)

# git 其他

## git add . 与 git add * 的区别    
- 1
git add . 按.gitignore规则全部提交。不会提示.gitignore
git add * 与add .  不一样的在于会提示已被忽略的内容。

![image.png](C:/Users/Lei Yu/OneDrive - bupt.edu.cn/Document/MarkDownImg/2qewiGLZths853p.png)
- 2
git add .默认添加. 开头的文件  例如.gitignore   git add * 忽略. 开头的文件

## git中出现 > 这个符号怎么退出？

ctrl + d 即可退出。

这个表示没有输入完成，输入没有闭合。比如，只输入了一边的双引号或单引号。
## 如何重置本项目用户信息：
```
git config user.name 'your name'
git config user.email xx@email.com
```
## git为本地分支设置对应的远程分支
当运行git pull时，如果本地分支没有绑定远程分支，将无法正常pull。
运行命令如下：
```
git branch --set-upstream-to=origin/branchName
```
## Git中用vim打开、修改、保存文件
一、vim 有两种工作模式： 
1.命令模式：接受、执行 vim操作命令的模式，打开文件后的默认模式； 

2.编辑模式：对打开的文件内容进行 增、删、改 操作的模式；

3.在编辑模式下按下ESC键，回退到命令模式；在命令模式下按i，进入编辑模式

二、创建、打开文件：

1.输入 touch 文件名 ，可创建文件。

2.使用 vim 加文件路径（或文件名）的模式打开文件，如果文件存在则打开现有文件，如果文件不存在则新建文件。 

3.键盘输入字母i进入插入编辑模式。

三、保存文件： 

1.在编辑模式下编辑文件 

2.按下ESC键，退出编辑模式，切换到命令模式。 

3.在命令模式下键入"ZZ"或者":wq"保存修改并且退出 vim。 

4.如果只想保存文件，则键入":w"，回车后底行会提示写入操作结果，并保持停留在命令模式。

四、放弃所有文件修改： 
1.放弃所有文件修改：按下ESC键进入命令模式，键入":q!"回车后放弃修改并退出vim。 

2.放弃所有文件修改，但不退出 vi，即回退到文件打开后最后一次保存操作的状态，继续进行文件操作：按下ESC键进入命令模式，键入":e!"，回车后回到命令模式。

五、查看文件内容：

在git窗口，输入命令：cat 文件名

 六、创建文件夹

在git窗口，输入命令：touch 文件夹名

## 使用Git上传项目时需要输入用户名和密码
### 小登陆框
这里简述一下主要步骤：

step1.将上传代码的方式从 https 改成 ssh
<img src="https://i.loli.net/2020/11/21/f6q1YtxQbuHjTUP.png" width = "30%" hight="30%" />
使用命令：`git remote set-url origin git@github.com:LawsonAbs/luogu.git` 后面的用户和项目名需要根据你自己的情况而改变。

step2.在自己本地生成ssh公钥并写在github中指定项目的key中
`ssh-keygen -rsa -C "LawsonAbs"` 生成LawsonAbs这个用户的公钥。

step3.执行命令 `git push -u origin master`

### 小登录

## Git自动push/pull

### windows

bat脚本

```shell
@echo off
e:
cd C:\Users\Lei Yu\OneDrive - bupt.edu.cn\Document\MarkDownNote
git config --global credential.helper store
git pull
```

将该脚本放到git仓库的根目录中

<img src="https://i.loli.net/2020/12/30/I5aPugKB8JsHyN7.png" alt="image-20201214140659729.png" style="zoom:50%;" />

<img src="https://i.loli.net/2020/12/30/MEGXt1kxI9jyzVH.png" alt="image-20201214140108699.png" style="zoom:50%;" />

参考：https://blog.csdn.net/yougou_sully/article/details/106114253

### Mac

利用[crontab](https://zh.wikipedia.org/wiki/Cron)编写定时脚本，自动同步（可以在macOS和Linux下运行）

```bash
crontab -e
```

输入下列定时任务（每15分钟同步一次）

```bash
*/15 * * * * cd /Users/yourname/Markdown;git add .;git commit -m "AutoSave";git push origin master
```

保存即可，可以用下列命令检验定时脚本是否设置成功。

```bash
crontab -l
```

## [git fetch & pull详解](https://www.cnblogs.com/runnerjack/p/9342362.html)

**1、 简单概括**

先用一张图来理一下git fetch和git pull的概念：

<img src="https://i.loli.net/2020/12/30/W5Gpohtq9JMXgSx.png" alt="image-20201216152342411.png" style="zoom:50%;" />

可以简单的概括为：

git fetch是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。

而git pull 则是将远程主机的最新内容拉下来后直接合并，即：git pull = git fetch + git merge，这样可能会产生冲突，需要手动解决。

下面我们来详细了解一下git fetch 和git pull 的用法。 

**2、分支的概念**

在介绍两种方法之前，我们需要先了解一下分支的概念： 

分支是用来标记特定代码的提交，每一个分支通过SHA1sum值来标识，所以对分支的操作是轻量级的，你改变的仅仅是SHA1sum值。

如下图所示，当前有2个分支，A,C,E属于master分支，而A,B，D,F属于dev分支。

```apl
A----C----E（master）
 \
  B---D---F(dev)
```

它们的head指针分别指向E和F，对上述做如下操作：

```bash
git checkout master  //选择or切换到master分支
 git merge dev     //将dev分支合并到当前分支(master)中
```

合并完成后：

```apl
A---C---E---G(master)
 \       \
  B---D---F（dev）
```

现在ABCDEG属于master，G是一次合并后的结果，是将E和Ｆ的代码合并后的结果，可能会出现冲突。而ABDF依然属于dev分支。可以继续在dev的分支上进行开发:

```apl
A---C---E---G---H(master)
 \       \
  B---D---F---I（dev）
```

分支（branch）的基本操作：

```bash
git branch //查看本地所有分支 

git branch -r //查看远程所有分支

git branch -a //查看本地和远程的所有分支

git branch <branchname> //新建分支

git branch -d <branchname> //删除本地分支

git branch -d -r <branchname> //删除远程分支，删除后还需推送到服务器
 git push origin:<branchname> //删除后推送至服务器

git branch -m <oldbranch> <newbranch> //重命名本地分支
 /**
 *重命名远程分支：
 *1、删除远程待修改分支
 *2、push本地新分支到远程服务器
 */

//git中一些选项解释:

-d
 --delete：删除

-D
 --delete --force的快捷键

-f
 --force：强制

-m
 --move：移动或重命名

-M
 --move --force的快捷键

-r
 --remote：远程

-a
 --all：所有
```

**3、git fetch** **用法**

git fetch 命令：

```bash
git fetch <远程主机名> //这个命令将某个远程主机的更新全部取回本地
```

如果只想取回特定分支的更新，可以指定分支名：

```bash
git fetch <远程主机名> <分支名> //注意之间有空格
```

最常见的命令如取回origin 主机的master 分支：

```bash
git fetch origin master
```

取回更新后，会返回一个FETCH_HEAD ，指的是某个branch在服务器上的最新状态，我们可以在本地通过它查看刚取回的更新信息：

```bash
git log -p FETCH_HEAD
```

如图： 

<img src="https://i.loli.net/2020/12/30/u6sKZoTQqXzhP1D.png" alt="clip_image002.png" style="zoom: 67%;" />

可以看到返回的信息包括更新的文件名，更新的作者和时间，以及更新的代码（19行红色[删除]和绿色[新增]部分）。

我们可以通过这些信息来判断是否产生冲突，以确定是否将更新merge到当前分支。 

 

**4、git pull** **用法**

前面提到，git pull 的过程可以理解为：

```bash
git fetch origin master //从远程主机的master分支拉取最新内容 
git merge FETCH_HEAD   //将拉取下来的最新内容合并到当前所在的分支中
```

即将远程主机的某个分支的更新取回，并与本地指定的分支合并，完整格式可表示为：

```bash
git pull <远程主机名> <远程分支名>:<本地分支名>
```

如果远程分支是与当前分支合并，则冒号后面的部分可以省略：

```bash
git pull origin next
```

# git 报错
## fatal: This operation must be run in a work tree 
可能原因： 1.路径不正确
          2.git仓库没有正确创建
## warning: LF will be replaced by CRLF in ****. The file will have its original line endings in y
警告原因：可能是你的项目使用了GitHub的开源项目，这个开源项目上传的环境（Linux）和你本次上传GitHub仓库的环境（Windows）不同
例如：本机上传的环境是win10    ，而在你本机项目中引用的GitHub项目上传环境是Linux

windows中的换行符为 CRLF，而在Linux下的换行符为LF

就会产生如上换行符不同的警告
但是这个错误可以直接忽略，对项目影响不大，我觉着如果以后项目在其他环境上运行的话，还可以通过这个方法改变换行符，是项目在其他环境中正常运行！

## Automatic merge failed; fix conflicts and then commit the result.
自动合并失败；修改冲突然后提交修改后的结果。
<<<<<<<< HEAD

         你写的代码

===============

          别人写的代码

>>>>>>>>>>>>>>> sdhqd128dqwenasjdq

## Another git process seems to be running in this repository

windows对于进程的同步互斥管理，是有资源上锁机制的。猜测这里肯定是有进程对某资源进行了加锁，但是由于进程突然崩溃，未来得及解锁，导致其他进程访问不了。
进入工作区目录下的隐藏文件.git，其中的index.lock文件删除掉，然后重新打开git bash进程，问题解决。

## git push错误failed to push some refs to解决方法

**原因**

当我们在git版本库中发现一个问题后，如你在git上对它进行了在线修改，但是没有对本地库进行同步（做到push之前，都先pull下代码，就可以保证本地库和远程库代码一致）。这个时候你再次commit，想把本地库提交到远程git库中，就会出现push失败问题。

> failed to push some refs to

**解决**

方法：1

跟因就是远程库与本地库代码不一致导致的，我们只要把远程库同步到本地库即可，使用如下命令：

> git pull --rebase origin master

指令意思就是把远程库中的跟新合并到本地库中（可能存在冲突需要解决），--rebase的作用是取消本地库中刚刚提交的commit，并把他们接到更新后的版本库中。


方法：2

或者使用如下命令，将commit的代码撤回，然后再git pull也行。

> git reset --soft HEAD^

## refusing to merge unrelated histories


I think you have commit in remote repository and when you pull this error happen.

use this command

```bash
git pull origin master --allow-unrelated-histories
git merge origin origin/master
```

i suggest reading at https://stackoverflow.com/questions/39761024/refusing-to-merge-unrelated-histories-failure-while-pulling-to-recovered-repos

This cannot be answered shortly.

**Warning**: You **should not** use the `--allow-unrelated-histories` flag unless you know what unrelated history is and are sure you need it. The check was introduced just to prevent disasters when people merge unrelated projects by mistake.

*As far as I understand*, in your case **has happened the following**:

You have cloned a project at some point `1`, and made some development to point `2`. Meanwhile, project has evolved to some point `3`.

![image.png](https://i.loli.net/2020/12/31/46K8hvcb2szfDei.png)

Then you for some reason lost your local .git subdirectory - which contained all your history from `1` to `2`. You managed to restore the current state though.

![image.png](https://i.loli.net/2020/12/31/lHrdIohY27LnQuR.png)

But now it does not have any history - it looks like the whole project has appeared out of nowhere. If you ask Git to merge them it will not be able to say where your changes are, so it can add them to remote project, as far as I understand it will just report massive add/add conflicts.

**You should** now find back that commit `1` from the remote history where you have cloned the project (I assume you did not pull after that; if you did then you should instead look for the last commit you have pulled). Then you should modify your history so that is starts from that commit 1, and then Git will be able to merge correctly (with pull for example).

So, the steps (assuming you are now in your restored commit without history):

- estimate where is the commit `1` you have cloned from as some `1?`, based on commit time for example
- run `git diff _1?_..HEAD`, and read carefully. Make sure that the difference contains only edits which you have made. If it contains more then you should have picked a bit wrong `1?` and need to adjust it and repeat this step
- after you have found the commit `1`. You should make it your parent; do `git --reset --soft _1_`, and then `git commit`.

![image.png](https://i.loli.net/2020/12/31/OY9r8AGXcPMBVUe.png)

Now it looks like you have cloned from `1`, then made one commit with all your changes. Your intermediate history is lost anyway with your older `.git` directory, but now you can run your `git pull` - it will merge correctly.

# .gitignore用法

```
# 忽略 .a 文件
*.a
# 但否定忽略 lib.a, 尽管已经在前面忽略了 .a 文件
!lib.a
# 仅在当前目录下忽略 TODO 文件， 但不包括子目录下的 subdir/TODO
/TODO
# 忽略 build/ 文件夹下的所有文件
build/
# 忽略 doc/notes.txt, 不包括 doc/server/arch.txt
doc/*.txt
# 忽略所有的 .pdf 文件 在 doc/ directory 下的
doc/**/*.pdf
```

[模板](https://github.com/github/gitignore)

# 其他git命令

1. 修改最近的提交
git commit --amend
Copy
-amend 参数允许追加修改至最后一次提交之后（比如忘记提交的文件），添加 -no-edit 参数将追加提交至最后一次提交之内并且不改变提交的备注信息。如果没有新的修改提交， -amend 参数将会重写最后一次提交的备注信息。

查看更多：git help commit

2. 交互式选择部分修改
git add -p
-p (或 -path) 会允许我们交互式的选择是否提交每一处改动，这样，每次提交就只包含一处修改，即 拆分我们的该次添加修改，每一处修改都有一个提交

查看更多: git help add

3. 交互式的处理储藏中文件的每个部分
git stash -p
跟 git-add 有点类似，你可以使用 --patch 选项交互式地选择每个跟踪文件的要储藏的部分。

通过  git help stash 命令了解更多。

4. 储藏未跟踪的文件
git stash -u
默认情况下，储藏命令是不包括未跟踪的文件的。为了储藏未跟踪的文件你需要使用 -u 参数。 -a (—all) 参数会将未跟踪和忽略的文件一起储藏，通常情况下，这可能不是你需要的。

5. 交互式地还原文件的选中部分
git checkout -p
--patch 参数还可以用于有选择的丢弃每个跟踪文件的某些部分。我给这个命令起了个别名叫 git discard。

使用 git help checkout 了解更多。

6. 切到上一个分支
git checkout -
该命令可以使你快速切换到之前签出的分支。一般来说 - 是上一个分支的别名。它可以跟别的命令一起使用。我将 checkout 简写成 co ，这样它就变成了  git co - 。

7. 还原本地所有修改
git checkout .
在确定可以放弃所有本地修改的情况下可以使用  . 立即去完成。好的习惯是使用 checkout --patch 命令。

8. 查看修改
git diff --staged
这个命令会显示已经暂存的文件和上次提交文件之前的差异，git diff 是显示还没有暂存起来的修改；当你使用 git add . 之后再使用 git diff 就会发现什么都没有。

使用 git help diff 了解更多。

9. 重命名本地分支
git branch -m old-name new-name
如果你想重命名当前所在的分支，可以将命令缩短为以下格式:

git branch -m new-name
查看更多: git help branch

10. 重命名远端分支
为了重命名远端分支，你一旦修改本地分支名称，就立即需要删除远端分支，再将重命名后的本地分支推送上去。

git push origin :old-name
git push origin new-name
11. 一次性查看所有冲突文件
变基可能会导致冲突，这个命令将打开所有需要你处理的冲突文件。

git diff --name-only --diff-filter=U | uniq  | xargs $EDITOR
12. 有哪些改变？
git whatchanged --since="2 weeks ago"
该命令将显示最近两周提交的简单日志，包括每次提交的介绍和修改的文件。下图是译者找的 demo：



13. 从最近一次提交中删除文件
假设你提交了一个错误的文件。你可以通过组合 rm 和 commit --amend 命令从上次提交中快速删除它：

git rm —-cached <file-to-remove>
git commit —-amend
14. 查找分支
git branch --contains <commit>
该命令将展示包含指定提交的所有分支。

15. 本地储藏优化
git gc --prune=now --aggressive



















































[^1]:这是解释

