=============
 django 笔记
=============

django form 技巧
================

form 初始化值 initial 参数的使用
--------------------------------

有两种方式为form 提供初始化值

在创建form对象时::

	form = JournalForm(initial={'tank': 123})

或者在定义form时::

	tank = forms.IntegerField(widget=forms.HiddenInput(), initial=123)


ChoiceField 选项初始化
----------------------

例如要生成一个 1985-2012年的 ChoiceField 可以这么做::

	choices = [(u'', u'Select Year')]
	choices.extend([(unicode(year), unicode(year)) for year in range(1985, 2013)])
	graduation = forms.ChoiceField(choices=choices)


ChoiceField 自定义 blankoption 默认值内容
-----------------------------------------

django 生成的 ChoiceField 会带有一个 -------- 的默认值, 它的 value 是 "" , 我们可以自定义这个值, 让提示更加友好::

	class ThingForm(models.ModelForm):
	    class Meta:
		model = Thing
	    def __init__(self, *args, **kwargs):
	    	super(ThingForm, self).__init__(*args, **kwargs)
		self.fields['year'].empty_label = _('Select Year...')

当然也可以直接在定义 form 的时候就加上这个值::

	Verb = ModelChoiceField(Verb.objects.all(), empty_label=None)


如果一个 view 中要处理多个 form 最好使用 prifx 参数
---------------------------------------------------

prifx 可以在view中区分不同的 form 方便分别处理::

	form = MyFormClass(prefix='some_prefix')
	form = MyFormClass(request.POST, prefix='some_prefix')


django template 技巧
====================

在django模版渲染中过滤HTML标签
------------------------------

Django内置的filter，有一个是removetags，可以过滤多个指定的Html标签，

比如博客的内容摘要可能是html格式的，显示的时候，去掉a p span div标签，可以这样写::


	{{blog.content|removetags:"a p span div"}}


removetags函数会去掉指定的标签，注意 | 两边都不能留空格。



django debug 技巧
=================

使用 ipdb 调试 django
---------------------
		
ipdb是pdb的加强版, 有类似于ipython的自动补全功能, 在Django中使用ipdb进行调试只需要在需要下断点的地方插入以下内容::

	import ipdb
	ipdb.set_trace()

刷新页面后, runserver 会停在设置断点的地方, 可以查看变量的值, 或者手动做一些别的操作.

常用命令::

	h -> help
	a -> argument

