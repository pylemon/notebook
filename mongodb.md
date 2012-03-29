# MongoDB the devinitive guide


### bacics

1. document 是MongoDB的基本组成单元.大约等同于关系型数据库中的一行数据  

2. collection 可以当作是一个无schema的表  

3. 一个MongoDB实例可以创建多个不相关的数据库. 每个数据库都有自己的collections 和 permissions.  

4. 提供了一个简单且强大的 JavaScript shell 用以管理和操纵数据.  

5. 每个 document 都有一个特殊的 "_id" key 并且这个 key 在这个 document 所属的 collection 中是唯一的.  


### documents  

一个 document 看起来类似 JavaScript 中的对象 或是 python 中的 dict  

```
{"greeting" : "Hello, world!"}
```

通常一个 document 保存的是多个 key/value 对, 与 python 中的 dict 不同的是它保存的数据是有序的, 也就是说  

```
{"greeting" : "Hello, world!" , "foo" : 3}
{"foo" : 3 , "greeting" : "Hello, world!" }
```

在一个 collection 中是不同的两个 document  


### keys and values  

*key 可以使用任意 utf-8 字符命名, 一般来说 key 就是一个字符串, 但是它有以下限制*

1. 不能出现 "\0" (null字符) 这个字符被用来标记一个 key 的结束

2. 不能使用 . 或 $ 他们被用于一些特殊属性中, 一般被认为是保留字

3. 以下划线开头的key 被认为是保留字；但并不是强制要求不能以_开始命名一个 key  


*所有的 key 都是大小写敏感的. foo 和 Foo 表示不同的 key*
```
{"foo" : 3}
{"Foo" : 3}
```

* value 所有的 value 都是类型敏感的, 下面的例子中的两个 document 存储的是不同的值
```
{"foo" : 3}
{"foo" : "3"}
```

*在一个 document 中 不能出现重复的 key*
```
{"greeting" : "Hello, world!", "greeting" : "Hello, MongoDB!"}
```


### collections  

*collections 是无schema的, 也就是说在同一个 collection 中 可以有不同结构甚至是不同长度的 document*
```
{"greeting" : "Hello, world!"}
{"foo" : 5}
```

*命名 collections 可以以任意 utf-8 字符来命名 但有以下限制*

1. 空字符串("")不允许作为一个 collection 的名称.  

2. 和 document 中的 key 一样, 不允许在名称中出现 "\0" . 它标志着一个 collection name 的结束.  

3. 不能创建任何以 "system." 开头的 collection , 这是一个系统保留的前缀.  
```
system.users       ->  包含了 database 的user信息
system.namespaces  ->  包含了所有 database collections 的信息
```

4. 不要在 collection name 中使用 $ , 一些数据库的驱动程序会自动生成一些包含 $ 符号的 collection, 除非你需要访问这些 collection 否则不要使用它


### subcollections

subcollections 是一个在MongoDB中组织数据的方式. 它是被极力推荐的. 但是 subcollection 并不表示 其中的 collections 有任何的联系. 例如下面有2个 collection
```
blog.post
blog.author
```
这两个 collrction 并无任何关系. 甚至 blog 这个 collection 都可以不存在. 推荐这样命名 是因为一些数据库驱动工具会提供相应的方法与接口 方便的访问到不同的 collection


### databases

一个MongoDB实例可以hold住多个databases , 一个 database 可以由多个 collections 组成, 
不同的 database 可以设置不同的权限, 每一个 database 在文件系统中会以一个独立文件的形式存在. 
推荐的做法是将同一个 app 下的数据放在一个 database 中, 便于管理. 

*database 的命名规则*

1. 不能使用 ""(空字符串) 作为 database name

2. 不能包含以下字符: " "(空白), "."(点), "$", "/", "\", "\0"

3. database name 应该全部为小写字母

4. database name 的长度不能超过 64 bytes.

有这些要求是因为 database 最终会以一个文件的形式存在与系统中.  

另外 database 还保留了几个 内置的 database name : admin, local, config
