# 基本概念  

1. document 是MongoDB的基本组成单元.大约等同于关系型数据库中的一行数据  

2. collection 可以当作是一个无schema的表  

3. 一个MongoDB实例可以创建多个不相关的数据库. 每个数据库都有自己的collections 和 permissions.  

4. 提供了一个简单且强大的 JavaScript shell 用以管理和操纵数据.  

5. 每个 document 都有一个特殊的 "_id" key 并且这个 key 在这个 document 所属的 collection 中是唯一的.  


## documents  

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


## keys and values  

* 可以使用所有 utf-8 字符集中的字符作为 key, key都是一个字符串

* 不能出现 "\0" (null字符) 这个字符被用来标记一个key的结束

* 不能使用 . 或 $ 他们被用于一些特殊属性中, 一般被认为是保留字

* 以下划线开头的key 被认为是保留字；但并不是强制要求不能以_开始命名一个key

* 所有的 key 都是大小写敏感的. foo 和 Foo 表示不同的 key

```
{"foo" : 3}
{"Foo" : 3}
```

* 所有的 value 都是类型敏感的 

```
{"foo" : 3}
{"foo" : "3"}
```

* 在一个 document 中 不能出现重复的 key

```
{"greeting" : "Hello, world!", "greeting" : "Hello, MongoDB!"}
```

## collections  

