# Git使用教程

## 简要介绍

Git是一个开源的分布式版本控制系统，用于高效的处理任何大小的项目。

以下是一些基本命令

```python
cd name 切换到目录下
cd  .. 回退到上一级目录
mkdir name 在当前目录下创建目录
touch name 会创建文件（如果文件已存在不会有影响），可以指定后缀
echo “XXX”>test.txt 将会把内容写入文件
echo “XXX”>>test.txt 会创建文件并写入
cat filename 可以显示文件内容
```



在强制命令行中退出使用:ESC+:+wq





## Git的工作流程

+ 克隆Git资源作为工作目录

+ 在克隆的资源上添加或者修改文件

+ 如果其他人修改了资源，可以选择更新资源

+ 在提交前查看修改

+ 提交修改

+ 修改完成后，如果发现错误可以撤回并再次修改

  

  

<img src="git_figure/image-20240513171758145.png" alt="image-20240513171758145" style="zoom:50%;" />

## Git中的区域

<img src="git_figure/image-20240513172607872.png" alt="image-20240513172607872" style="zoom:50%;" />



+ 图中左侧为工作区，右侧为版本库，其中标记有index的是暂存区，标记有master是master分支所代表的目录树，HEAD相当于一个指向master的指针。
+ object标识的区域为Git的对象库，，里面包含了各种创建的对象。
+ 当对工作区修改或新增文件的时候，暂存区的目录树会被更新，同时变化的文件内容会被存储到对象库中一个新的对象中，该对象的ID会被记录到暂存区的索引中。
+ 当执行提价操作时，暂存区的目录树会被写到对象库中，同时master所指的分支会更新为暂存区的目录树。
+ 当执行git reset HEAD操作时暂存区的目录树会被重写，背master所指的目录树替换，工作区不受影响。
+ 当执行git rm -cached<file>指令时，会从暂存区删除文件，而工作区不变。
+ 当执行git checkout,git checkout -<file>时，会用暂存区的全部会指定的文件替换工作区的文件。
+ 当执行git checkout HEAD,git checkout HEAD<file>时,会用master分支中全部或指定的文件替换工作区以及暂存区的所有文件。

Git的工作就是创建和保存项目的快照以及与之后的快照进行对比。

<img src="git_figure/image-20240513175356847.png" alt="image-20240513175356847" style="zoom:50%;" />



## Git基本操作

> | `git init`  | 初始化仓库                             |
> | ----------- | -------------------------------------- |
> | `git clone` | 拷贝一份远程仓库，也就是下载一个项目。 |
>
> 提交与修改
>
> | `git add`                           | 添加文件到暂存区，git add .会添加所有的修改 |
> | ----------------------------------- | ------------------------------------------- |
> | `git status`                        | 查看仓库当前的状态，显示有变更的文件。      |
> | `git diff`                          | 比较文件的不同，即暂存区和工作区的差异。    |
> | `git commit`                        | 提交暂存区到本地仓库。                      |
> | `git reset`                         | 回退版本。                                  |
> | `git rm`                            | 将文件从暂存区和工作区中删除。              |
> | `git mv`                            | 移动或重命名工作区文件。                    |
> | `git checkout`                      | 分支切换。                                  |
> | `git switch （Git 2.23 版本引入）`  | 更清晰地切换分支。                          |
> | `git restore （Git 2.23 版本引入）` | 恢复或撤销文件的更改。                      |
>
> 提交日志
>
> | `git log`          | 查看历史提交记录                     |
> | ------------------ | ------------------------------------ |
> | `git blame <file>` | 以列表形式查看指定文件的历史修改记录 |
>
> 远程操作
>
> | `git remote` | 查看当前的远程仓库 |
> | ------------ | ------------------ |
> | `git fetch`  | 从远程获取代码库   |
> | `git pull`   | 下载远程代码并合并 |
> | `git push`   | 上传远程代码并合并 |







## 创建仓库

通过命令git init 会初始化当前目录为一个git仓库，git init newrepo会将newrepo会初始化指定的目录为git仓库。



在初始化后目录中会出现名为.git的目录，所有Git所需的数据和资源都会存放在这个目录下。

如果当前目录下有几个文件想要纳入版本控制，需要先用git add命令让Git对这些文件进行跟踪，然后再提交。

```C
git add *.c 
git add README
git commit -m "初始化项目版本"
```



> 将当前目录下以c结尾的和名为readme的文件提交到仓库中,注意引号要是双引号



克隆项目

```python
git clone  repo
git clone repo  directory
```

> 克隆现有的项目，克隆到指定的位置





设置配置

```python
git config -list
git config -e    
git config -e --global   
git config --global user.name "runoob"
git config --global user.email test@runoob.com
```

> 显示当前配置
>
> 规定后面的修改是对于当前仓库的
>
> 规定后面的操作是对于所有仓库的
>
> 对全体仓库修改用户名
>
> 对全体仓库修改邮箱

下图展示了配置中有哪些参数

![image-20240513184233244](git_figure/image-20240513184233244.png)



## 分支管理

使用分支表示可以从开发主线分离开，然后在不影响主线的情况下继续工作。

<img src="git_figure/image-20240513184423855.png" alt="image-20240513184423855" style="zoom:50%;" />

对一个目录操作的过程中，不断切换分支，目录下对应的文件也是在不断变化的：a分支下的test.txt文件和b分支下的test.txt文件是不同的，改变其中一个不会影响到另一个。





```python
git branch 列出分支，默认下会有master分支，列出的分支中前方带有星号表示当前的所处的分支

git branch name 创建分支 ，在当前位置常见的分支会包含有原位置的资源副本

git branch -M name 重命名当前分支

git checkout name 切换分支，当切换分支时，Git会使用该分支最后提交的快照替换当前的工作目录中的内容，所以多个分支不需要多个目录

git checkout -b name  创建并立即切换到新分支

git branch -d name 删除分支

git merge name 将指定分支合并到当前分支，这里合并的是修改，如果a中有a.c b中有a.c，b把其中的a.c删除后，合并到a中，最后a里面是不会有a.c的因为在b里面把他删除了
```





在使用merge是会出现冲突，比如说两个人对同一个文件的同一个位置修改了，合并时就会出现冲突。

对于冲突的位置Git会选择让用户自己决定取舍：



在文件中会被如图所示的方式排版，注意多余的标识符要删干净

<img src="git_figure/image-20240518222358985.png" alt="image-20240518222358985" style="zoom:50%;" />



在解决冲突之后要追踪修改

## 查看提交历史

git log [选项] [分支名/提交哈希]



选项如下，注意选项可以结合使用：

> - `--oneline`：以简洁的一行格式显示提交信息
> - `--graph`：以图形化方式显示分支和合并历史
> - `--author=<作者>`：只显示特定作者的提交 git log --author=Linus --oneline -5 名字不需要加引号。-5表示输出最符合的5条
> - `--since=<时间>`：只显示指定时间之后的提交  git log --oneline --before={3.weeks.ago} --after={2010-04-18} 
> - `--until=<时间>`：只显示指定时间之前的提交



git blame [选项] <文件路径>

选项如下：

> - `-L <起始行号>,<结束行号>`：只显示指定行号范围内的代码注释。
> - `-C`：对于重命名或拷贝的代码行，也进行代码行溯源。
> - `-M`：对于移动的代码行，也进行代码行溯源。
> - `-C -C` 或 `-M -M`：对于较多改动的代码行，进行更进一步的溯源。

## 标签

有点类似于给软件打个v1.0版本一样

git tag 查找所有的标签

git tag -a name [-m "XXXX"] 给最近的一次提交打上标签

git tag -d name 删除标签



git show tagname 查看此版本所修改的内容





## Git远程仓库

<img src="git_figure/image-20240520220842588.png" alt="image-20240520220842588" style="zoom:50%;" />

如果想童通过git分享代码或者合作，那么就需要将数据放到一台其他人员能连接到的服务器上。







用于添加一个远程仓库到本地并命名为origin，其中XXX是在github上创建的远程仓库的SSH协议用于关联

<img src="git_figure/image-20240521180631780.png" alt="image-20240521180631780" style="zoom:50%;" />

```python
git remote add origin XXX
```





将本地的 `main` 分支推送到远程仓库的 `main` 分支，并设置跟踪关系。

```python
git push -u origin main
```

这里的-u表示设置关联关系，，这样之后在分支中push和pull时可以直接简写git push/pull 远程仓库名 [分支名]，分支名可以省略





从远程仓库的 `main` 分支拉取代码并合并到本地的 `main` 分支

```python
git pull origin main
```





在本地仓库做了修改，使用git push origin推到远程仓库上，在远程仓库上做了修改git push origin获取修改并合并 



git remote rm 仓库名 用于删除远程仓库
