==========
 Git 笔记
==========

在 github 中使用 service hooks
==============================

本站使用 `shpinx` 自动生成 html 格式的笔记文件. 由于采用了 http://readthedocs.org/ 的服务,

`RTD` 支持 `service hooks` 服务, 可以直接在 `github` 中设置下, 这样在 `push` 的时候, 自动重新 build 文档. 确实方便.

在 `github` 项目的 `admin` 页面. 找到 `Service hooks` 选中 `ReadTheDocs` , `active` , 然后 `update`

这样以后在每次提交新的笔记后, `RTD` 就会自动的去 `github` 取最新的代码, 然后重新 `build` html页面了.


