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




在 github 中使用 service hooks
==============================

本站使用 `shpinx` 自动生成 html 格式的笔记文件. 由于采用了 http://readthedocs.org/ 的服务,

`RTD` 支持 `service hooks` 服务, 可以直接在 `github` 中设置下, 这样在 `push` 的时候, 自动重新 build 文档. 确实方便.

在 `github` 项目的 `admin` 页面. 找到 `Service hooks` 选中 `ReadTheDocs` , `active` , 然后 `update`

这样以后在每次提交新的笔记后, `RTD` 就会自动的去 `github` 取最新的代码, 然后重新 `build` html页面了.


