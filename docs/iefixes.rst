=========================
 IE fixes css javascript
=========================

console.log
===========

console.log 可以很方便的在console中输出日志. 但是 IE8 貌似不支持这个. 

会报告 console 未定义的错误. 目前没想到很好的解决办法. 如果相关js需要在ie下运行 只能注释掉所有 console.log


String.trim() 不支持IE8
=======================

`String.trim()` 在 IE8 下会得到一个错误
(Object doesn't support this property or method)
google了一番, SO上有人提到可以使用 jQuery 的 `$.trim(String)` 来替代这个方法.


li 标签缩进错误
===============

这个问题貌似在 IE8 IE9 firefox 下都存在, 具体表现为 所有的 LI 标签都会有个默认的 margin-left:40px;

使用 iefixes.css 修复这个问题::

    li {
        margin-left: -40px;
    }
      

