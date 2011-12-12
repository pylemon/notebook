### django在命令行下执行的脚本使用ORM ###

项目中遇到一个东西需要写一个deamon来处理，需要在deamon脚本中使用到djangoORM来操作数据库，

这样就需要在deamon script中初始化一个django environment
	
```python
    # settings.py
    from os.path import join
    import os.path
    settings_path = os.path.abspath(os.path.dirname(__file__))


    # deamon.py
    import sys
    import settings
    from django.core.management import setup_environ
    sys.path.append(settings.settings_path)
    setup_environ(settings)
```

这样就可以在deamon中为所欲为了。哈哈。

	
### 在django模版渲染中过滤HTML标签 ###

Django内置的filter，有一个是removetags，可以过滤多个指定的Html标签，

比如博客的内容摘要可能是html格式的，显示的时候，去掉a p span div标签，可以这样写

	{{blog.content|removetags:"a p span div"}}

removetags函数会去掉指定的标签，注意 | 两边都不能留空格。


### django模版中的截断过滤器 ###

想输出一段摘要，需要用到截断过滤器，查阅了官方文档发现*truncatewords*方法

但是这个方法只能按照词【空格】 来截断需要的内容。不能按照字符长度截断。官方竟然没有提供相关的功能。

	For example:

	{{ value|truncatewords:2 }}
	If value is "Joel is a slug", the output will be "Joel is ...".

	Newlines within the string will be removed.
	
	{{ value|truncatewords_html:2 }}
	If value is "<p>Joel is a slug</p>", the output will be "<p>Joel is ...</p>".
	
谷歌一下后发现stackoverflow上有解决办法。下面是一个取巧的办法。

	{{ value|slice:"5" }}{% if value|length > 5 %}...{% endif %}
	
一行代码就搞定了。很方便。另外一个办法就是创建一个自定义的template filter

	from django import template
	register = template.Library()

	@register.filter("truncate_chars")
	def truncate_chars(value, max_length):
		if len(value) > max_length:
			truncd_val = value[:max_length]
			if not len(value) == max_length+1 and value[max_length+1] != " ":
				truncd_val = truncd_val[:truncd_val.rfind(" ")]
			return  truncd_val + "..."
		return value
		
