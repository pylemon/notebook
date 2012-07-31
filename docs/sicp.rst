sicp 读书笔记
=============

Lisp 与 Scheme
--------------

Lisp 一直在变化， scheme 作为 Lisp 的一门方言是从 Lisp 演化而来。

在 linux 下安装 scheme 解释器。

.. note::

    guile-2.0 - GNU extension language and Scheme interpreter

可以使用 guile

.. code-block:: bash

    $ sudo apt-get install guile-2.0

主要的不同之处
--------------

* 变量约束的静态作用域，允许函数产生出函数作为值。

* 采用100个函数在一种数据结构上操作，远远优于用10个函数在10种数据结构上操作。


第一章习题
----------

1.1 表达式求值
~~~~~~~~~~~~~~

.. code-block:: scheme

    10,12,8,3,10 6,a,b,19,#f,4,16,6,16


1.2 表达式转换前缀式
~~~~~~~~~~~~~~~~~~~~

.. code-block:: scheme

    (/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5)))))
       (* 3 (- 6 2) (- 2 7)))


1.3 定义一个过程求三个数中最大的两个数的和
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: scheme

    (define (max x y) (if (< x y) y x))
    (define (min x y) (if (< x y) x y))
    (define (func x y z)
            (+ (max x y) (max (min x y) z)))


1.4 (define (a-plus-abs-b a b) ((if (> b 0) + -) a b)) 是表达的什么？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: scheme

    a + |B's|


1.5 应用序求值 正则序求值
~~~~~~~~~~~~~~~~~~~~~~~~~
 
