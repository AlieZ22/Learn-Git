# Git学习

@author: zzmine

@time: 2019.11.16

Git 是一个分布式版本管理系统，是为了更好地管理Linux内核开发而创立的

Git 可以在任何时间点，把文档的状态作为更新记录保存起来，因此可以把编译过的文档复原到以前的状态，也可以显示编辑前后的内容差异。

## 概述

### 历史记录数据库

数据库(repository)，是记录文件或目录状态的地方，存储着内容修改的历史记录。

#### 远程数据库和本地数据库

- 远程数据库：配有专用的服务器，为了多人共享而建立的数据库
- 本地数据库：为了方便个人用户使用，在自己机器上配置的数据库

![Image text](https://github.com/AlieZ22/test/blob/master/images/1.png)

#### 创建数据库

创建本地数据库有两种方法：

1. 创建全新的数据库
2. 复制远程数据库

#### 修改记录的提交

若要把文件或目录的添加和变更保存到数据库，就要进行提交。执行提交后，数据库中会生成上次提交的状态与当前状态的差异记录（称为revision）

![Image text](https://github.com/AlieZ22/test/blob/master/images/2.png)

系统会根据修改的内容计算出没有重复的40位英文及数字来给提交命名。指定这个命名，就可以在数据库中找到对应的提交。

### 工作树和索引

在git管理下，实际操作的目录被称为工作树。

数据库和工作树之间有索引，索引是为了向数据库提交做准备的区域。

![Image text](https://github.com/AlieZ22/test/blob/master/images/3.png)

说明：Git在执行提交的时候，不是直接将工作树的状态保存到数据库，而是将索引的状态保存到数据库。因此，要提交文件，首先需要把文件加入到索引区域中。

## 入门篇-实际操作

### Git基础操作

#### 1，安装Git

使用命令行版，于 http://git-scm.com/ 处下载Git Bash， 命令行中输入git --version查看版本

#### 2，初期设定

安装后，输入用户名和电子邮件。该设置在安装Git后进行一次即可，这些信息将作为提交者信息显示在更新历史中

Git的设定被存放在用户本地目录的 .gitconfig档案里。在这里我们使用命令来操作：

```
$ git config --global user.name <用户名>
$ git config --global user.email <电子邮件>
```

![Image text](https://github.com/AlieZ22/test/blob/master/images/4.png)

#### 3，创建数据库

创建新的本地数据库，创建一个名称为tutorial的空目录，并把它放在Git的管理置下。命令行移至tutorial目录下之后，输入：

```
$ git init
```

表示**将该目录移动到本地Git数据库**

#### 4，提交文件

在tutorial目录中新建一个文件，然后将文件添加到数据库中。

首先在tutorial目录里新建一个名为sample.txt的文本文件，在文件中输入一些内容。

使用status命令确认工作树和索引的状态：

```
$ git status
```

![Image text](https://github.com/AlieZ22/test/blob/master/images/5.png)

从status响应中，我们可以看到“sample.txt”目前不是历史记录对象。所以首先需要把它加入到索引，就可以追踪它的变更了。

**将文件加入到索引，要使用add命令（git add <file>）**。在<file>指定加入索引的文件。用空格分割可指定多个文件。Tips: 使用git add . 可以把该目录下所有文件加入索引。

现在，试着将sample.txt加入到索引，然后确认一下工作树和索引的状态：

```
$ git add sample.txt
$ git status
```

![Image text](https://github.com/AlieZ22/test/blob/master/images/6.png)

从该状态中，我们可以看出sample.txt已经加入到索引了，那么我们就可以提交文件了。输入以下命令可以进行文件的提交：

```
$ git commit -m "first commit"
```

![Image text](https://github.com/AlieZ22/test/blob/master/images/7.png)

再次确认工作树和索引状态：

![Image text](https://github.com/AlieZ22/test/blob/master/images/8.png)

表示工作树上，没有新的变更需要提交。

使用log命令，我们可以在数据库的提交记录中看到新的提交。

![Image text](https://github.com/AlieZ22/test/blob/master/images/9.png)

或者使用 git log --graph --oneline

![Image text](https://github.com/AlieZ22/test/blob/master/images/10.png)

修改sample.txt文件，使用diff命令，可以查看修改的内容

![Image text](https://github.com/AlieZ22/test/blob/master/images/11.png)

### 共享数据库

前面的内容均是在本地数据库中操作的，下面介绍如何在远程数据库上共享本地数据库的修改记录。

#### 1，push(推送)到远程数据库

为了将本地数据库的修改记录共享到远程数据库，必须上传本地数据库中存储的修改记录。为此，需要在Git上执行Push(推送)操作。

在执行Push之后，本地的修改记录会被上传到远程数据库。所以远程数据库的修改记录就会和本地数据库的修改记录保持同步。

![Image text](https://github.com/AlieZ22/test/blob/master/images/12.png)

详细操作见步骤4

#### 2，clone(克隆)远程数据库

进行Clone操作，就可以复制远程数据库。远程数据库的全部内容都会被下载，之后在另一台机器的本地数据库上就可以操作了。

![Image text](https://github.com/AlieZ22/test/blob/master/images/13.png)

使用clone指令，可以复制数据库，在<repository>指定远程数据库的URL，在<directory>中指定新目录的名称

```
$ git clone <repository> <directory>
// 如： git clone https://github.com/AlieZ22/test tutorial2
```

结果如下：（可以在任意目录下直接git clone, 没必要在有.git的目录下）

![Image text](https://github.com/AlieZ22/test/blob/master/images/14.png)

#### 3，从远程数据库pull(拉取)

若共享的远程数据库是由多人同时作业，那么作业完毕后所有人都要把修改推送到远程数据库。然后，自己的本地数据库也需要更新其他人推送的变更内容。

拉取(Pull)操作就可以把远程数据库的内容更新到本地数据库。

进行Pull操作，就是从远程数据库下载最近的变更日志，并覆盖自己本地数据库的相关内容。

![Image text](https://github.com/AlieZ22/test/blob/master/images/15.png)

使用pull指令进行拉取操作，省略数据库名称的话，会默认在origin的数据库中进行pull

```
$ git pull <repository> <refspec> ...
// 如：git pull origin master
```



#### 4，在Github建立远程数据库

登陆到Github，进入自己的repositories，新建一个repository

![Image text](https://github.com/AlieZ22/test/blob/master/images/16.png)

可以给远程数据库取一个别名。这样，下次推送的时候就不需要输入长串的远程数据库地址了。我们将远程数据库命名为“origin”

**使用remote指令添加关联的远程数据库**。在<name>处输入远程数据库名称，在<url>处指定远程数据库URL:

```
$ git remote add <name> <url>      （这种是以https协议进行访问的）
$ git remote set-url origin git@github.com:someaccount/someproject.git （添加SSH后以git协议访问，好处是git pull, git push 不用输入密码）
```

##### 拓展-如何在github上配置ssh公钥

1. 创建一个新的ssh key

   ```
   $ ssh-keygen -t rsa -C "your_example@example.com"
   ```

2. 按提示，可以不输入密码，直接回车

3. 会在默认文件夹下生成两个密钥文件 <id_rsa> 和 <id_tsa.pub>

4. 在github上，右上方setting，添加SSH Key，title任意取，内容复制<id_tsa.pub>文件中的内容

5. 测试ssh key

   ```
   ssh -T git@github.com
   ```

   至此，完成ssh公钥配置。

https 与 ssh 的区别：

1. 前者可以随意克隆github上的项目，而不管是谁的；而后者则是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。
2. https url 在push的时候是需要验证用户名和密码的；而 SSH 在push的时候，是不需要输入用户名的，如果配置SSH key的时候设置了密码，则需要输入密码的，否则直接是不需要输入密码的。

如：(注意，**这个地址要从clone or download处复制，有.git后缀**)

```
$ git remote add origin https://github.com/AlieZ22/test.git
// 其他remote指令
$ git remote remove <name>    （name 比如origin）
$ git remote -v          （显示当前与本地数据库关联的远程数据库）
```

注意：执行推送或者拉取的时候，如果省略了远程数据库的名称，则默认使用名为”origin“的远程数据库。因此一般都会把远程数据库命名为origin。

**使用push命令向远程数据库推送更改内容**。<repository>处输入目标地址，<refspec>处指定推送的分支。

```
$ git push <repository> <refspec> ...
```

例如：

```
$ git push -u origin master
```

注意：第一次推送master分支时加了 -u 参数，表示将本地master分支与远程master分支关联起来。之后本地做了修改需要提交，只需要git push origin master就可以了，而不需要再加 -u 参数了。

##### 解惑-产生的一个小问题

问题描述：本地无法push到远程

![Image text](https://github.com/AlieZ22/test/blob/master/images/17.png)

原因：在github上创建远程数据库时，选择了init README.md， 导致在本地无法push项目到远程，因为git发现远程的README.md 文件并没有存在于本地，所以报错了。

解决：可以通过合并远程和本地的方式，让本地也包含README.md 文件

```
$ git pull --rebase origin master
```

运行后会发现tutorial文件夹下多了README.md文件，此时再push到远程就可以了！

#### 5，整合修改记录

注意：在执行pull之后，进行下一次push之前，如果有其他人进行了推送内容到远程数据库的话，那么你的push将被拒绝！
![Image text](https://github.com/AlieZ22/test/blob/master/images/18.png)

这种情况下，在读取别人push的变更并进行合并操作之前，你的push都将被拒绝。这是因为，如果不进行合并就试图覆盖已有的变更记录的话，其他人push的变更（图中的提交C）就会丢失。

***实例：使用tutorial 与 tutorial2 制造冲突**

修改tutorial中的sample.txt内容如下：

```txt
Hello Git!
Question: how to pull the history document?
test conflict 1
```

修改tutorial中的sample.txt内容如下：

```txt
Hello Git!
Question: how to pull the history document?
test conflict 2
```

检查tutorial2目录此时的工作树和索引状态，将修改的sample.txt 加入索引，commit提交到本地数据库，push到远程数据库。

为tutorial项目执行同tutorial2一样的操作，发现在push到远程数据库时，push被拒绝了。原因跟之前push出错一样。
![Image text](https://github.com/AlieZ22/test/blob/master/images/19.png)

为了把变更内容推送到远程数据库，我们必须手动解决冲突。首先运行pull，从远程数据库取得最新的变更记录。

```
$ git pull origin master
```

![Image text](https://github.com/AlieZ22/test/blob/master/images/20.png)

该图显示了合并时发生冲突的信息 [ Merge conflict in sample.txt ]

运行后，发现tutorial目录下的sample.txt文件内容变成了：

```txt
Hello Git!
Question: how to pull the history document?
<<<<<<< HEAD
test conflict 1
=======
test conflict 2
>>>>>>> 88754172fbf8f5c355133c2df368a1a28adff2fc
```

这说明，git已经添加标示以显示冲突的部分。手动修改时，导入对方的修改，并删除多余的标示以解决冲突，修改sample文件如下：

```
Hello Git!
Question: how to pull the history document?
test conflict 1
test conflict 2
```



## 高级篇-进阶原理

### 分支(branch)

在开发软件时，可能存在多个release版本，并且需要对各个版本进行维护。Git的分支功能可以支持**同时进行多个功能的开发和版本管理**。

分支 是为了将修改记录的整体流程分叉保存，分叉后的分支不受其他分支的影响，所以在同一个数据库里可以同时进行多个修改。

![Image text](https://github.com/AlieZ22/test/blob/master/images/21.png)

分叉的分支可以合并。

有时候，为了不受其他开发人员的影响，可以在主分支上建立自己专用的分支。完成工作后，将自己分支上的修改合并到主分支。

![Image text](https://github.com/AlieZ22/test/blob/master/images/22.png)

在数据库进行最初的提交后，Git会创建一个名为**master的分支**。因此之后的提交，在切换分支之前都会添加到master分支里。

#### Merge分支

Merge分支是为了可以随时发布release而创建的分支，它还能作为Topic分支的源分支使用。

保持分支稳定的状态是很重要的。如果要进行更改，通常先创建Topic分支，而针对该分支，可以使用Jenkins之类的CI工具进行自动化编译以及测试。

通常，大家会将master分支当作Merge分支使用。

#### Topic分支

Topic分支是为了开发新功能或修复Bug等任务而建立的分支。若要同时进行多个任务，可创建多个Topic分支。

Topic分支是从稳定的Merge分支创建的。完成作页后，要把Topic分支合并回Merge分支。

![Image text](https://github.com/AlieZ22/test/blob/master/images/23.png)

#### 分支的切换

若要切换作业的分支，就要进行checkout操作。进行checkout时，git会从工作树还原向目标分支提交的修改内容。checkout之后的提交记录将被追加到目标分支。

**HEAD** 指向的是现在使用中的分支的最后一次更新。通常默认指向master分支的最后一次更新。通过移动HEAD，就可以变更使用的分支。

![Image text](https://github.com/AlieZ22/test/blob/master/images/24.png)

**stash** 是临时保存文件修改内容的区域。stash可以在那时保存工作树和索引里还没有提交的修改内容，事后再取出暂存的修改，应用到原来的分支或其他分支上。

还未提交的修改内容以及新添加的文件，留在索引区域或工作树的情况下 切换到其他分支时，修改内容会从原来的分支移动到目标分支。

但是如果在checkout的目标分支中 相同的文件也有修改，checkout会失败。这时要么先提交修改内容，要么**用stash暂时保存修改内容后在checkout**。

![Image text](https://github.com/AlieZ22/test/blob/master/images/25.png)

#### 分支的合并

完成作业后的topic分支，最后要合并回merge分支。合并分支有两种方法：使用merge 或 rebase。使用这两种方法，合并后分支的历史记录会有很大的差别。

**merge方法**  可以合并多个历史记录

如下图所示，bugfix分支是从master分支分叉出来的	

![Image text](https://github.com/AlieZ22/test/blob/master/images/26.png)

合并bugfix分支到master分支时，若master分支的状态没有再被修改过了，那么合并很简单，就是把master分支位置移动到bugfix的最新分支上，Git就会合并。这样的合并称为fast-forward(快速)合并。

![Image text](https://github.com/AlieZ22/test/blob/master/images/27.png)

但是，若master分支，在分出bugfix之后还有新的更新，这种情况下，要把master分支的修改内容和bugfix分支的修改内容汇合起来。合并两个修改内容生成一个新的提交，这是master分支的HEAD会移动到该提交上。

![Image text](https://github.com/AlieZ22/test/blob/master/images/28.png)

![Image text](https://github.com/AlieZ22/test/blob/master/images/29.png)

**rebase方法** 

和merge方法的例子一样，若使用rebase方法合并分支，它的历史记录将比较简单。

![Image text](https://github.com/AlieZ22/test/blob/master/images/30.png)

![Image text](https://github.com/AlieZ22/test/blob/master/images/31.png)

参考：https://nvie.com/posts/a-successful-git-branching-model/

#### 实战-分支

##### 1，建立分支

使用branch命令来创建分支

```
$ git branch <branchname>
// 如：git branch issue1
```

##### 2，切换分支

执行checkout命令退出当前分支，进入目标分支

```
$ git checkout <branch>
// 如：git checkout issue1
```

![Image text](https://github.com/AlieZ22/test/blob/master/images/32.png)

##### 3，合并分支

执行merge命令以合并分支

```
$ git merge <commit>
// 如：切换到master分支中执行git merge issue1
```

##### 4，删除分支

既然issue1分支的内容已经顺利地合并到master分支了，现在可以将其删除了。使用以下命令进行删除：

```
$ git branch -d <branchname>
// 如：git branch -d issue1
```

issue1分支被删除了，可以使用branch命令来确认是否已经删除

```
$ git branch
* master
```



### 远程数据库

#### pull

执行pull可以取得远程数据库的历史记录，下面对实现细节进行说明。

首先确认更新的本地数据库没有任何改变：

![Image text](https://github.com/AlieZ22/test/blob/master/images/33.png)

此时只是执行fast-forward合并。其中master是本地数据库的master分支，origin/master是远程数据库origin的master分支。

![Image text](https://github.com/AlieZ22/test/blob/master/images/34.png)

如果本地数据库的master分支有新的历史纪录，就需要合并双方的修改：

![Image text](https://github.com/AlieZ22/test/blob/master/images/35.png)

执行pull就可以进行合并。这是，如果没有冲突的修改，就会自动创建合并提交。如果发生冲突的话，就要先解决冲突，再手动提交。（详见入门篇-共享数据库-5,整合修改记录）

![Image text](https://github.com/AlieZ22/test/blob/master/images/36.png)

#### fetch

执行pull，远程数据库的内容就会自动合并。但是，有时候只是想确认远程数据库的内容而不想合并到本地。这种情况下，就可以使用fetch.

使用fetch就可以取到远程数据库的最新历史记录，取得的提交会导入到没有名字的分支，这个分支可以从名为FETCH_HEAD的地方退出。

![Image text](https://github.com/AlieZ22/test/blob/master/images/37.png)

这种情况下，若要把远程数据库合并到本地，就可以合并FETCH_HEAD，或重新执行pull。

注意：**其实pull的本质就是fetch+merge**

#### push

从本地数据库push到远程数据库时，要fast-forward合并push的分支。如果发生冲突，push会被拒绝。 可以 先把远程数据库pull回来然后用rebase合并，再push。

![Image text](https://github.com/AlieZ22/test/blob/master/images/38.png)

#### 标签

标签是为了更方便地参考提交而给它表上易懂的名称。

Git 可以使用两种标签：轻标签和注解标签。打上的标签是固定的，不能像分支一样移动位置。

1. 轻标签：添加名称
2. 注解标签：添加名称；添加注解；添加签名

一般情况下，发布标签是采用注解标签来添加注解或签名的。而轻标签是为了在本地暂时使用或是一次性使用。

![Image text](https://github.com/AlieZ22/test/blob/master/images/39.png)

##### 添加轻标签

使用tag命令来添加标签，在<tagname>中输入标签的名称

```
$ git tag <tagname>
// 如：git tag apple
```

接着可以显示标签列表：

```
$ git tag
apple
```

如果在log命令后添加 --decorate选项执行，就可以显示包含标签资料的历史记录：

![Image text](https://github.com/AlieZ22/test/blob/master/images/40.png)

此时的工作树状态为：

![Image text](https://github.com/AlieZ22/test/blob/master/images/41.png)

##### 添加注解标签

若要添加注解标签，可以在tag命令后指定 -a 选项执行。执行后会启动编辑区，输入注解（vim语法，按i后插入字符，ESC退出插入状态，:wq保存退出），也可以指定 -m 选项来添加注解。

```
$ git tag -a <tagname>
// 如：git tag -a banana
```

在显示标签的指令后指定 -n 选项，可以显示标签和注解：

![Image text](https://github.com/AlieZ22/test/blob/master/images/42.png)

##### 删除标签

在tag 指令后添加 -d 选项，指定<tagname>删除标签

```
$ git tag -d <tagname>
```



### 后续学习：

1，Linux下常用的文本编辑器  Vim 以及 Nano

使用Nano较为便利，方法是使用Ctrl结合其他按键进行

2，cat <filename> 可便利地查看当前目录文件内容

若想查看stage中的文件可使用：

参考：https://www.jianshu.com/p/abca119649b5

```
$ git ls-files     // 显示stage中所有目录/文件
$ git ls-files -s   //显示stage中的目录/文件，同时显示对应的Blob对象
$ git cat-file -p 6cef   // 通过Blob对象，查询文件中的内容
```

3，如果log显示的历史信息太长了，发现退不出去了，可以按q键退出

4，关于版本回退：分三种情况讨论

- 未加入索引：使用checkout <filename> 可检出，即用stage中的内容覆盖当前工作区的内容

- 通过add加入到索引中了：使用git reset <filename> 可重置索引中的file

  ```
  // 类似的，若错误的add了不想加入的文件还可以这样操作，来撤回add
  git reset HEAD 如果后面什么都不加，那么就是撤销上一次add的全部内容
  git reset HEAD <filename> 对某个文件的add操作撤销
  ```

- 通过commit提交了： 使用如下几种硬重置指令可以回退

  ```
  $ git reset --hard HEAD^     // 回退到上一个版本
  $ git reset --hard HEAD~n    // 回退到前n个版本
  $ git reset --hard <commitID>  // 回退到某个特定版本
  
  // 其中，可以通过git reflog 查看历史的所有提交
  // 注意，若在回退到之前的版本后，使用git log查看历史提交，将不会显示该版本之后提交的版本，此时只有使用git reflog才能查看后续版本
  ```

  

------未完
