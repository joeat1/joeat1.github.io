---
layout: wiki
title: 爬虫设计
categories: wiki 爬虫
description: 爬虫设计
keywords: 爬虫设计
---

> 爬虫组件基本包含 爬虫调度器，URL管理器（已爬与待爬），HTML下载器（获取网络信息），HTML解析器，数据存储器五个部分。一般需要经过网站调研分析，数据获取，数据提取等步骤。

## 0x1 网站调研 ##
> 了解网站基本特点，设计相应的爬取方案
> 探测反爬措施，设计对应反反爬方式

+ 查看网站的robot.txt与sitemap，了解网站限制内容
	+ `User-agent`: 指定对哪些爬虫生效
	+ `Disallow`: 指定不允许访问的网址
	+ `Allow`: 指定允许访问的网址
+ 识别网站使用的技术（`builtwith`），了解内容大概以什么形式加载
+ 估计网站相应页面的规模（利用Google的`site:XXX`搜索相应网站目录下的返回结果数量）
+ 了解网站所有者
+ 查看网站结构，了解网站数据交互的接口方式，确定目标数据的位置和返回结构
+ 分析网站访问的身份控制与源控制的方法（如header与cooike，`Referer`与`Host`）
  + 两次重复 登陆/注册/访问 某一个页面，对比两次访问的信息区别，找到变化的与不变的参数，针对每一个参数寻找获取的方法，部分不变的参数有可能可以不添加；
  + 一些页面信息可能包含了需要提交的参数数值；一些前面的请求返回信息将提供给之后请求的参数，但是可能参数名变化而参数值不变；一些参数在js代码中函数被生成（可以使用pyV8执行JS函数直接获得）。
+ 查看移动端（可通过修改agent来实现）呈现的网站网页
+ 查看网站提供的API接口以及返回的信息内容
+ 简单爬取尝试，探测网站存在的反爬策略，设计爬取策略
+ 动态生成页面内容可能导致爬虫认为未处理而持续停留在一个页面，陷入爬虫陷阱，需要设置最大深度。
+  根据网站动态交互的方式，选择使用单网页下载或浏览器自动渲染的方式
+ 注意事项
  +  整个过程中需要保证编码维持为utf-8
  + 正则表达式的使用：(.*?)用于获取匹配位置的括号匹配的信息
  + 使用xpath时候，可以结合chrome浏览器的元素copy到xpath功能。（在copy中还可以复制源码，确定选择器），复制要完整，尤其注意网页上类与ID中出现空格之类的东西。
    `//定位根节点位置，/向下寻找，/text()获取文本信息，/@XXX选择元素`
    `导入模块From lxml import etree后使用Selector = etree.HTML(XXX) 转换成xpath识别的文本类型。在使用content = selector.xpath(‘//div[@id]/ul’)获取文本信息。`
    如果出现同一个标签下需要的元素的ID具有相同开头，则使用(‘//div[start-with(@id,”test)]/text())可以实现目的。但是直接使用/text()会使得标签中套的标签不能被获取。此时需要先大后小：data = selector.xpath(‘//div[@id]’)后使用content = data.xpath(‘string(.)’)

## 0x2 数据提取 ##
> 推荐使用lxml（解析速率比beautifulsoup快）
> 对经常分析的更新速率慢的网站建议进行网页缓存，以便于增加或调整需要提取的数据
> 分析同一URL的数据是否有不同，是否存在增量式爬取

+ 数据库准备，推荐使用[MongoDB](http://www.mongodb.org/downloads) 网页/数据去重

+ 设计数据库结构

+ 定位网页元素，提取目标数据 推荐使用[lxml](http://lxml.de/) 

+ 分析网页中的动态内容（AJAX方式，GWT方式）

+ 根据网站数据交互的URL，调整参数，以获得更多的信息

+ 智能化解析工具

+ 使用`urlretrieve`函数实现多媒体文件抽取,并设置回调函数
  ```python
  import urllib
  def schedule(blocknum, blocksize, totalsize):
      per = 100.0 * blocknum * blocksize / totalsize
      if per > 100:
          per = 100
      print("[+] Now get: %d" % per)
  
  urllib.urlretrieve(img_url, filename, schedule)
  ```
  
+ 使用集合set的方式进行内存去重，可以使用数据库去重，使用缓存数据库Redis去重，
可以在此基础上使用MD5处理URL取中间28位字符作为比对信息，可以将未爬取的已爬的信息序列化存储，保存信息以备后续使用。

+ 增量爬取信息一定要掌握去重的工作！！！

## 0x3 反反爬虫措施 ##

+ 验证码处理：图片识别OCR、 人工打码API、 模拟鼠标滑动
+ 速率限制：随机休眠(time.sleep)
+ IP限制：使用代理 `requests.get(URL, procies={'http': 'http://IP.port'}, 'https': 'http://IP.port'})` 个人使用，60-70个代理IP即可。
+ 用户访问限制：培育多个账户
+ 字体反爬机制，即网站定义了字体文件，然后进行相应的查找替换，在前端看起来，是没有任何差异的。

## 0x4 加速下载 ##
> 爬取同一个网站下的网页，一般两次下载间间隔至少1s
> celery 专用的信息传输工具

+ 多线程
+ 多进程

## 0x5 APP数据采集 ##
### 模拟器+burpsuit ###
+ 修改模拟器中无线网络，长按后添加代理，设置为本机ip
+ 在burpsuit中设置Proxy Listeners添加监听端口信息
+ 使用模拟器的浏览器连接到代理的端口，并下载证书，也可以直接通过bp生成后给模拟器
+ 将文件后缀der修改为cer，设置->安全->从SD卡安装证书 

### PC端热点+wireshark ###
+ PC端创建无线热点
+ 手机端连接，开启wireshark分析相关文件

----
## scrapy ##
> 参考：https://scrapy-chs.readthedocs.io/zh_CN/0.24/intro/tutorial.html
### 基本流程 ###
1. 创建工程
2. 定义提取项
3. 写爬虫获取并提取数据
4. 写pipeline存储提取的数据

### 基本使用 ###

```python
scrapy startproject tutorial   创建工程
    scrapy.cfg: the project configuration file   #是这个工程的根目录所在层
    tutorial/: the project’s python module, you’ll later import your code from here.
    tutorial/items.py: the project’s items file.  # 定义要爬取存储的项
    tutorial/pipelines.py: the project’s pipelines file.
    tutorial/settings.py: the project’s settings file.  #包含工程的基本设置信息
    tutorial/spiders/: a directory where you’ll later put your spiders.

from scrapy.item import Item, Field 每个Item有多个Field对象
class DmozItem(Item):
title = Field()

scrapy genspider name <domain>  创建爬虫  爬虫的文件名不能与项目名字重合

#爬虫定义
from scrapy.spider import BaseSpider
name: identifies the Spider.
start_urls: is a list of URLs where the Spider will begin to crawl from.
• parse() is a method of the spider, which will be called with the downloaded Response object of each start URL. The response is passed to the method as the first and only argument.

scrapy crawl mininova -o scraped_data.json -t json  输出为json格式
scrapy crawl mininova -o teachers.csv               输出为csv格式
scrapy crawl mininova

爬虫运行前，关闭setting里面的robots协议 ROBOTSTXT_OBEY

需要存储时需要开启ITEM_PIPELINES
ITEM_PIPELINES = {
   'test_taobao.pipelines.TestTaobaoPipeline': 300,
}
启动 DOWNLOADER_MIDDLEWARES 也是类似的，其后的数值是执行的先后顺序，从低到高

shell方式尝试 用于调试
scrapy shell "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/"
```

## BeautifulSoup ##
> 从HTML或XML文件中提取数据的Python库
> 参考 `https://www.cnblogs.com/haiyan123/p/8317398.html`

### 基本使用 ###
```
from bs4 import BeautifulSoup
soup=BeautifulSoup(html_doc,'lxml') #具有容错功能
soup.find_all(name="a")
soup.find_all(text="The Dormouse's story"))  #按照文本来找
```

## xpath ##
> Chrome 给我们提供了一键获取 xpath 地址的方法（右键->检查->copy->copy xpath）
### 基本使用 ###
+ / 从根节点选取。
+ //    从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。
+ . 选取当前节点。
+ ..    选取当前节点的父节点。
+ @   选取属性。 标签[@属性名='属性值']
+ 提取标签里面的内容，表达式： //text()
+ 包含HTML标签的所有文字内容提取：string() "string(//div[@class='post-content'])"
+ //td: selects all the <td> elements
+ /html/head/title/text(): selects the text inside the aforementioned <title> element.
+ //div[@class="mine"]: selects all div elements which contain an attribute class="mine"
+ /bookstore/book[price>35.00]  选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。
+ /bookstore/book[position()<3] 选取最前面的两个属于 bookstore 元素的子元素的 book 元素。
+ /bookstore/book[last()]    选取属于 bookstore 子元素的最后一个 book 元素。


## CSS ##

### 基本使用 ###
+ ".class"
+ "#id"
+ `*`  所有元素
+ :optional 用于匹配可选的输入元素
+ :link a:link  选择所有未访问链接
+ :visited  a:visited   选择所有访问过的链接