==========
 Git 笔记
==========

git log
=======

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
==========

* 更改上一次的提交内容
  
  有时候在做了 commit 操作后，发现上一次的提交内容有问题，有部分文件忘了提交。这种情况可以使用 --amend 来更改上一次的提交
  一个典型的使用场景，将 readme.md 添加到上一次提交中去。 ::
    
    git add readme.md
    git ci --amend

  这样就可以将 readme.md 合并到上一次的提交中去了，最终只产生一个提交记录。


git reset
=========

* 还原已经提交的文件

  如果一个文件已经 add 到 stage 中，而我们发现它又是不需要的。这时可以采用 reset 命令，重置它。::

    git add .   
    git reset HEAD readme.md

  这样这个文件就会取消他的 stage 状态。


git checkout
============

* 放弃修改，还原文件

  如果一个文件想要还原到他修改前的样子。可以使用 checkout 命令。::

    git checkout -- readme.md

  这个命令会放弃所有作出的修改，将文件还原成变更前的状态。

git remote
==========

* 查看远程分支 ::
  
    liwei@liwei-E40:~/Notes(master⚡) » git remote -v
    origin	git@github.com:pylemon/notebook.git (fetch)
    origin	git@github.com:pylemon/notebook.git (push)

* 添加一个远程的源
  要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 git remote add [shortname] [url] ::

    git remote add pb git://github.com/paulboone/ticgit.git

  通过 git fetch pb 可以从这个分支上获取到更新

git fetch
=========

* 从远程仓库获取数据 ::

    git fetch [remote-name]


在 github 中使用 service hooks
==============================

本站使用 `shpinx` 自动生成 html 格式的笔记文件. 由于采用了 http://readthedocs.org/ 的服务,

`RTD` 支持 `service hooks` 服务, 可以直接在 `github` 中设置下, 这样在 `push` 的时候, 自动重新 build 文档. 确实方便.

在 `github` 项目的 `admin` 页面. 找到 `Service hooks` 选中 `ReadTheDocs` , `active` , 然后 `update`

这样以后在每次提交新的笔记后, `RTD` 就会自动的去 `github` 取最新的代码, 然后重新 `build` html页面了.


