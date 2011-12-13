# 使用Git管理代码 #
## 建立代码仓库 ##

*init config*

`$ git init`

当前目录被称为*工作树*


`$ git config --global user.name "Your Name Comes Here"`

`$ git config --global user.email you@yourdomain.example.com`

设置用户名和邮箱，此信息会保存在每次commit的log中，使用git log察看


## 常用操作 ##

*add commit revert*

`$ git add .`

将当前目录生成一个快照放到一个临时存储区域，创建索引


`$ echo "zh" > .gitignore`

`$ git add .`

忽略zh目录，不要放到当前缓存中更新

有关 gitignore 文件的诸多细节知识可阅读其使用手册: `$ man gitignore`


`$ git commit`

将索引中的代码提交，可选参数-m 'messages'可以增加备注信息，

如果不指定则调用默认的编辑器编辑message


`$ git revert filename`

从代码库中恢复某个文件

## 查看日志 ##

*log show*

`$ git log`

查看项目的更新记录

`$ git log --stat --summary`

查看每一次版本的大致变动情况


项目版本的更新细节:

`$ git show dfb02e6e4f2f7b573337763e5c0013802e392818`

后面的值为版本号

除了使用完整的版本号查看项目版本更新细节之外,也还可以使用以下方式:

`$ git show dfb02 # 一般只使用版本号的前几个字符即可`

`$ git show HEAD # 显示当前分支的最新版本的更新细节`

每一个项目版本号通常都对应存在一个父版本号,也就是项目的前一次版本状态。

可使用如下命令查看当前项目版本的父版本更新细节:

`$ git show HEAD^ # 查看 HEAD 的父版本更新细节`

`$ git show HEAD^^ # 查看 HEAD 的祖父版本更新细节`

`$ git show HEAD~4 # 查看 HEAD 的祖父之祖父的版本更新细节`

## 还原代码库 ##

*reset*

将代码库还原至某个版本

git-reset 命令有三个选项:--mixed 、 --soft 和 --hard 。我们在日常使用中仅使用前两个选项;

第三个选项由于杀伤力太大,容易损坏项目仓库,需谨慎使用。

--mixed 是 git-reset 的默认选项,它的作用是重置索引内容,将其定位到指定的项目版本,而不改变你的

工作树中的所有内容,只是提示你有哪些文件还未更新。

--soft 选项既不触动索引的位置,也不改变工作树中的任何内容,但是会要求它们处于一个良好的次序之内。

该选项会保留你在工作树中的所有更新并使之处于待提交状态。

## 协同开发 ##

*clone pull push*

`$ cd work`

`$ git clone lyr@192.168.0.7:~/work/m2ge m2ge`

通过ssh访问远端的某个git目录 将工作树复制到本地，如果不指定文件夹则和远端一致。

ssh 协议： *账户@IP:工作树路径*

clone之后 就在本地创建了一个工作目录，所有的提交删除工作都可以在本地进行，不需要频繁操作主分支


`$ git clone lyr@192.168.0.7:~/work/m2ge`

... 项目开发 ...

`$ git add` 增加改动的文件

`$ git commit` 向本地代码库提交

`$ git pull`  获取远端代码

... 解决版本合并问题 ...

`$ git push` 提交代码到远端库

git的代码是分布式管理的。所以每个机器上都保存了完整的版本库代码。


建立一个等价库

`$ mkdir -p ~/project/m2ge.git`

`$ cd ~/project/m2ge.git`

`$ git --bare init --shared`


## 分支管理 ##

*branch checkout merge*

`$ git branch`
察看当前分支

`$ git branch local`
建立一个分支

`$ git checkout local`
切换分支为local， *进行开发工作add commit ...*

`$ git checkout master`
将当前分支切换为master

`$ git merge local`
将local分支与当前分支合并

`$ git branch -d local`
删除local分支

**没有完成merge的分支是无法直接删除的，需要用到-D参数强制删除**
