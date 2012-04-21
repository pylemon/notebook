### tips django在命令行下执行的脚本使用ORM ###

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

	
### tips 在django模版渲染中过滤HTML标签 ###

Django内置的filter，有一个是removetags，可以过滤多个指定的Html标签，

比如博客的内容摘要可能是html格式的，显示的时候，去掉a p span div标签，可以这样写

```html
	{{blog.content|removetags:"a p span div"}}
```

removetags函数会去掉指定的标签，注意 | 两边都不能留空格。


### tips django模版中的截断过滤器 ###

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
		
		

### tips 使用ipdb调试django 
ipdb是pdb的加强版, 有类似于ipython的自动补全功能, 在Django中使用ipdb进行调试只需要在需要下断点的地方插入以下内容
```python
import ipdb
ipdb.set_trace()
```  
常用命令
h -> help
a -> argument



### form 初始化值

有两种方式为form 提供初始化值

在创建form对象时::

	form = JournalForm(initial={'tank': 123})

或者在定义form时::

	tank = forms.IntegerField(widget=forms.HiddenInput(), initial=123)


### ChoiceField 选项初始化

例如要生成一个 1985-2012年的 ChoiceField 可以这么做::

	choices = [(u'', u'Select Year')]
	choices.extend([(unicode(year), unicode(year)) for year in range(1985, 2013)])
	graduation = forms.ChoiceField(choices=choices)


### ChoiceField 自定义 blankoption 默认值内容

django 生成的 ChoiceField 会带有一个 -------- 的默认值, 它的 value 是 "" , 我们可以自定义这个值, 让提示更加友好

	class ThingForm(models.ModelForm):
		class Meta:
			model = Thing
		def __init__(self, *args, **kwargs):
			super(ThingForm, self).__init__(*args, **kwargs)
			self.fields['year'].empty_label = _('Select Year...')

当然也可以直接在定义 form 的时候就加上这个值

	Verb = ModelChoiceField(Verb.objects.all(), empty_label=None)


### 如果一个 view 中要处理多个 form 最好使用 prifx 参数

	form = MyFormClass(prefix='some_prefix')

	form = MyFormClass(request.POST, prefix='some_prefix')
