==========
 Git 笔记
==========


GitHub 开发流程
===============

为 GitHub上的项目提交 patch 的基本流程

初始化设置
----------

1. 在 GitHub 上 Fork 项目（点击 Fork 按钮）
2. Clone 项目代码到电脑上 : git clone git@github.com:«自己的github帐号»/«项目名称».git
3. 进入项目目录: cd «项目名称»
4. 设置源版本库(remote upstream): git remote add -f upstream git://github.com/«你folk的github帐号»/«项目名称».git

增加一个功能
------------

1. 为新功能新建一个分支: git checkout -b my_new_feature
2. 在新建的分支上工作，开发，提交。

新建一个分支并不是必须的，但是这会让你更容易在合并功能到主分支之后删除掉本地你的开发分支。方便的与主分支中真正的最新版本做diff，并且能够提交多个针对不同功能的 pull requerst

提交更新
--------

1. 将本地分支 Push 到 GitHub 上: git push origin my_new_feature
2. 生成 pull request: 在 GitHub 上点击 Pull Resquest 按钮

常用命令
--------

如果在你修改完新功能后，远端的主分支上已经发生了很多修改，你应该在这些新的修改上 `回放` 你本地作出的修改，这个应当用到 git rebase 命令。::

    git fetch upstream
    git rebase upstream/master

fetch 会将远端主分支的代码取回到本地，在做这个操作前需要确保本地没有尚未 commit 的内容。如果确有，可以用 git stash save 来暂存起来，稍后用 git stash pop 来弹出暂存的内容。
使用 fetch rebase 的操作流程有一个明显优于 merge 的优势， 它会给出一个非常清晰的提交图，并且他会 `修剪` 掉任何同时发生在你本地和远端源上的提交。

You can use -i with rebase for an “interactive” rebase. This allows you to drop, re-arrange, merge, and reword commits, e.g.:
你可以使用 -i 参数，在 rebase 的过程中可以用到交互式模式，这允许你删除，修改，合并或者回滚你的提交，像这样 ::

    git rebase -i upstream/master


使用Git管理代码
===============

建立代码仓库
------------

*init config* ::

    $ git init

当前目录被称为*工作树* ::

    $ git config --global user.name "Your Name Comes Here"

    $ git config --global user.email you@yourdomain.example.com

设置用户名和邮箱，此信息会保存在每次commit的log中，使用git log察看


常用操作
--------

*add commit revert* ::

    $ git add .

将当前目录生成一个快照放到一个临时存储区域，创建索引 ::

    $ echo "zh" > .gitignore
    $ git add .

忽略zh目录，不要放到当前缓存中更新

有关 gitignore 文件的诸多细节知识可阅读其使用手册: `$ man gitignore` ::

    $ git commit

将索引中的代码提交，可选参数-m 'messages'可以增加备注信息，

如果不指定则调用默认的编辑器编辑message ::

    $ git revert filename

从代码库中恢复某个文件


查看日志
--------

*log show* ::

    $ git log

查看项目的更新记录 ::

    $ git log --stat --summary

查看每一次版本的大致变动情况

项目版本的更新细节:

    $ git show dfb02e6e4f2f7b573337763e5c0013802e392818

后面的值为版本号(某一个版本的sha1值)

除了使用完整的版本号查看项目版本更新细节之外,也还可以使用以下方式 ::

    $ git show dfb02 # 一般只使用版本号的前几个字符即可
    $ git show HEAD # 显示当前分支的最新版本的更新细节

每一个项目版本号通常都对应存在一个父版本号,也就是项目的前一次版本状态。

可使用如下命令查看当前项目版本的父版本更新细节::

    $ git show HEAD^ # 查看 HEAD 的父版本更新细节
    $ git show HEAD^^ # 查看 HEAD 的祖父版本更新细节
    $ git show HEAD~4 # 查看 HEAD 的祖父之祖父的版本更新细节


还原代码库
----------

*reset*

将代码库还原至某个版本

git-reset 命令有三个选项:--mixed 、 --soft 和 --hard 。我们在日常使用中仅使用前两个选项;

第三个选项由于杀伤力太大,容易损坏项目仓库,需谨慎使用。

--mixed 是 git-reset 的默认选项,它的作用是重置索引内容,将其定位到指定的项目版本,而不改变你的

工作树中的所有内容,只是提示你有哪些文件还未更新。

--soft 选项既不触动索引的位置,也不改变工作树中的任何内容,但是会要求它们处于一个良好的次序之内。

该选项会保留你在工作树中的所有更新并使之处于待提交状态。


协同开发
--------

*clone pull push* ::

    $ cd work
    $ git clone lyr@192.168.0.7:~/work/m2ge m2ge

通过ssh访问远端的某个git目录 将工作树复制到本地，如果不指定文件夹则和远端一致。

ssh 协议： *账户@IP:路径*

clone之后 就在本地创建了一个工作目录，所有的提交删除工作都可以在本地进行，不需要频繁操作主分支

项目开发
~~~~~~~~

*  $ git add    增加改动的文件
*  $ git commit 向本地代码库提交
*  $ git pull   获取远端代码
*  $ git push   提交代码到远端库

git的代码是分布式管理的。所以每个机器上都保存了完整的版本库代码。

建立一个等价库 ::

    $ mkdir -p ~/project/m2ge.git
    $ cd ~/project/m2ge.git
    $ git --bare init --shared


分支管理
~~~~~~~~

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


git 常用命令
============

git log
-------

* 查看最近两次的提交内容 ::

    git log -p -2

* 查看提交的统计数据 ::

    git log --stat

* 查看最近两次的提交 ::

    git log -n 2
    git log -2

* 查看某个日期之前/之后的提交 after(since) & before(until) ::

    git log --after 2.weeks
    git log --before 2.years

* 查看某个作者或者提交者的提交日志作者和提交者还是有一点区别的 ::

    git log --author pyLemon
    git log --committer pyLemon

* 在提交的 log 内容中 grep 查找 ::

    git log --grep 'change'

* gitk 图形化的 git log 工具


git commit
----------

* 更改上一次的提交内容

  有时候在做了 commit 操作后，发现上一次的提交内容有问题，有部分文件忘了提交。这种情况可以使用 --amend 来更改上一次的提交
  一个典型的使用场景，将 readme.md 添加到上一次提交中去。 ::

    git add readme.md
    git ci --amend

  这样就可以将 readme.md 合并到上一次的提交中去了，最终只产生一个提交记录。


git reset
---------

* 还原已经提交的文件

  如果一个文件已经 add 到 stage 中，而我们发现它又是不需要的。这时可以采用 reset 命令，重置它。::

    git add .
    git reset HEAD readme.md

  这样这个文件就会取消他的 stage 状态。


git checkout
------------

* 放弃修改，还原文件

  如果一个文件想要还原到他修改前的样子。可以使用 checkout 命令。::

    git checkout -- readme.md

  这个命令会放弃所有作出的修改，将文件还原成变更前的状态。

git remote
----------

* 查看远程分支 ::

    liwei@liwei-E40:~/Notes(master⚡) » git remote -v
    origin	git@github.com:pylemon/notebook.git (fetch)
    origin	git@github.com:pylemon/notebook.git (push)

* 添加一个远程的源
  要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 git remote add [shortname] [url] ::

    git remote add pb git://github.com/paulboone/ticgit.git

  通过 git fetch pb 可以从这个分支上获取到更新

git fetch
---------

* 从远程仓库获取数据 ::

    git fetch [remote-name]


git cherry-pick
---------------

* cherry-pick 的作用是在当前分支上应用某一个提交，在合并的时候特别有用。

当前在 fixbug 分支上，做了3次修改并提交到本地 ::

     liwei@pylemon ⮀ ~/emacs ⮀ ⭠ fixbug ⮀ gla
     * 33eef50 - (HEAD, fixbug) add3 (2 seconds ago) <pylemon>
     * 41fdc68 - add 2 (17 seconds ago) <pylemon>
     * 4b48192 - add 1 (25 seconds ago) <pylemon>
     * 2041623 - (origin/master, origin/HEAD, master) fixes auto-complete (10 hours ago) <pylemon>

这时候我发现 add 1 这个提交是不需要的，之需要将后2次合并进主分支即可。可以这么操作::

     liwei@pylemon ⮀ ~/emacs ⮀ ⭠ master ⮀ git cherry-pick 4b48192..33eef50
     [master 32d478e] add 2
      0 files changed
       create mode 100644 2
     [master e488f01] add3
      0 files changed
       create mode 100644 3
     liwei@pylemon ⮀ ~/emacs ⮀ ⭠ master ⮀ ls
     2  3  auto-complete  my-emacs-config.el  site-lisp  snippets  tomorrow-night-theme.el  tools
     liwei@pylemon ⮀ ~/emacs ⮀ ⭠ master ⮀ gla
     * e488f01 - (HEAD, master) add3 (5 seconds ago) <pylemon>
     * 32d478e - add 2 (5 seconds ago) <pylemon>
     | * 33eef50 - (fixbug) add3 (5 minutes ago) <pylemon>
     | * 41fdc68 - add 2 (5 minutes ago) <pylemon>
     | * 4b48192 - add 1 (5 minutes ago) <pylemon>
     |/
     * 2041623 - (origin/master, origin/HEAD) fixes auto-complete (10 hours ago) <pylemon>

可以看到，执行 `cherry-pick` 后， git 将指定的 commit (不含第一个，包含最后一个) 应用到了 当前的 master 分支。
执行完之后，可以删除掉 `fixbug` 分支。这样就可以很方便的从一个分支里面选取一部分提交合并到主分支里面去了。当然同样也会遇到
需要merge的情况。和merege一样操作即可。


在 github 中使用 service hooks
==============================

本站使用 `shpinx` 自动生成 html 格式的笔记文件. 由于采用了 http://readthedocs.org/ 的服务,

`RTD` 支持 `service hooks` 服务, 可以直接在 `github` 中设置下, 这样在 `push` 的时候, 自动重新 build 文档. 确实方便.

在 `github` 项目的 `admin` 页面. 找到 `Service hooks` 选中 `ReadTheDocs` , `active` , 然后 `update`

这样以后在每次提交新的笔记后, `RTD` 就会自动的去 `github` 取最新的代码, 然后重新 `build` html页面了.
