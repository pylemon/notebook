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


应用序求值 与 正则序求值
------------------------

通常情况下他们得到的结果是一样的，有写特殊情况下，会得出不同的结果，见习题1.5

* 应用序求值 -> **先求值参数而后应用**

* 正则序求值 -> **完全展开而后归约会**

lisp 采用的是应用序求值，部分原因是这样可以避免对部分表达式重复求值从而提高效率。


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

    a 加 （b的绝对值）


1.5 应用序求值 正则序求值
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: scheme

    (define (p) (p))
    (define (test x y)
            (if (= x 0)
                0
                y))


而后求值下列表达式::

    (test 0 (p))

首先 (define (p) (p)) 定义了一个永远返回自己的递归死循环。直接调用 (p) 命令行就死掉了。

如果是正则序::

    首先替换 (test 0 (p)) 中的 test 然后再计算整个列表的值。因此替换后相当于是
    (if (= 0 0) 0 (p))
    而 if 的条件为 true 因此返回的是0 掠过了(p)这个死循环

如果是应用序求值::

    首先需要计算 (test 0 (p)) 中每个参数的值。然后再带入 test 中求值。因此，在计算参数的过程中，
    程序会陷入死循环。

在 guile 中测试的结果是。 程序进入了死循环。


1.6 使用 cond 实现的 new-if 遇到的问题 (牛顿法求平方根)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* 使用牛顿法求平方根

.. code-block:: scheme

    (define (average x y)
            (/ (+ x y) 2))

    (define (improve guess x)
            (average guess (/ x guess)))

    (define (square x)
            (* x x))

    (define (good-enough? guess x)
            (< (abs (- (square guess) x)) 0.001))

    (define (sqrt-iter guess x)
            (if (good-enough? guess x)
                 guess
                 (sqrt-iter (improve guess x) x)))

    (define (sqrt x)
            (sqrt-iter 1.0 x))

    scheme@(guile-user)> (sqrt 1)
    $3 = 1.0
    scheme@(guile-user)> (sqrt 2)
    $4 = 1.41421568627451


* 使用 cond 语法实现的 new-if

.. code-block:: scheme

    (define (new-if predicate then-clause else-clause)
            (cond (predicate then-clause)
                  (else else-clause)))

* 它看起来工作的很不错::

    scheme@(guile-user)> (new-if (= 2 3) 0 5)
    $1 = 5
    scheme@(guile-user)> (new-if (= 1 1) 0 5)
    $2 = 0

* 使用 new-if 的后果

.. code-block:: scheme

    (define (sqrt-iter-new guess x)
            (new-if (good-enough? guess x)
                     guess
                     (sqrt-iter-new (improve guess x) x)))

    (define (sqrt-new x)
            (sqrt-iter-new 1.0 x))

    scheme@(guile-user)> (sqrt-new 2)
    <unnamed port>:21:11: In procedure good-enough?:
    <unnamed port>:21:11: Throw to key `vm-error' with args `(vm-run "VM: Stack overflow" ())'.
    Entering a new prompt.  Type `,bt' for a backtrace or `,q' to continue.

得到了上面的错误信息。backtrace后发现貌似陷入了无限递归。
