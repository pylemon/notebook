### 使用Python logging模块记录deamon进程的日志 ###

* 简单用法

```python
import logging
logging.basicConfig(  
    level = logging.DEBUG,   # 生产环境应该使用ERROR级
    format = '%(asctime)s %(levelname)s %(module)s.%(funcName)s Line:%(lineno)d %(message)s',  
    filename = 'filelog.log',  #日志存放的目录
)

try:
    logging.info('log some info')
except:
    logging.debug('This is a debug message')
'''
	

* 比较正式一点的做法

```python
import logging
LEVELS = {
	'debug': logging.DEBUG,
	'info': logging.INFO,
	'warning': logging.WARNING,
	'error': logging.ERROR,
	'critical': logging.CRITICAL,
}
level = LEVELS.get('debug',logging.NOTSET)
logging.basicConfig(level = level)
logging.debug('This is a debug message')
'''

### 使用HTMLParser去除HTML源文件中的标签和属性 ###

在一些情况下，比如输出一片文章的摘要，前面的若干个字，但是存储在数据库中的文章又
偏偏是带有格式的HTML。这种情况下，可以使用下面这种方法将HTML中所有的标签剔除干净。

```python
from HTMLParser import HTMLParser

def strip_tags(html):
    html=html.strip()
    html=html.strip("\n")
    result=[]
    parse=HTMLParser()
    parse.handle_data=result.append
    parse.feed(html)
    parse.close()
    return "".join(result)

if __name__ == "__main__":
    html = """<a name="val">123</a><input type="text" name="afdsa" /><b><br><u>
fffffff<br></u></b><div style="text-align: left;"><b><u>fdafd</u></b><br><br></div>
"""
    print strip_tags(html)
'''
