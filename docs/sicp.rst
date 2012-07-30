sicp 读书笔记
=============

Lisp 与 Scheme
--------------

Lisp 一直在变化， scheme 作为 Lisp 的一门方言是从 Lisp 演化而来。  

在 linux 下安装 scheme 解释器。  

* `guile-2.0 - GNU extension language and Scheme interpreter`  

可以使用 guile ::

    $ sudo apt-get install guile-2.0

主要的不同之处
--------------

* 变量约束的静态作用域，允许函数产生出函数作为值。

* 采用100个函数在一种数据结构上操作，远远优于用10个函数在10种数据结构上操作。


第一章 构造过程抽象
-------------------
