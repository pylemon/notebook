==============================
 IE 兼容性问题 CSS JavaScript
==============================

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

IE and Opera set the list's indention with margin-left.  Firefox and, I 
believe, Konqueror/Safari use padding-left.  40px worth, to be exact.

do; ::

    ul {
         margin: 0;
         padding: 0;
    }

to remove all indention.


cross browser CSS opacity 跨浏览器CSS透明处理
=============================================

IE 和一些旧的浏览器 对透明的处理方式不一样, 解决方案 ::

  .transparent_class {
      /* IE 8 */
      -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=50)";
    
      /* IE 5-7 */
      filter: alpha(opacity=50);
    
      /* Netscape */
      -moz-opacity: 0.5;
    
      /* Safari 1.x */
      -khtml-opacity: 0.5;
    
      /* Good browsers */
      opacity: 0.5;
  }

