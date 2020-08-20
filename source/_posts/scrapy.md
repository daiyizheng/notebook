---
title: scrapy
date: 2020-06-01 10:17:44
summary: 一个快速、高层次的屏幕抓取和web抓取框架，用于抓取web站点并从页面中提取结构化的数据
tags:
  - 数据采集
  - 数据处理
categories: 爬虫
---

# scrapy

### scrapy官方文档

> https://scrapy-chs.readthedocs.io/zh_CN/0.24/index.html



##  scrapy的概念

**Scrapy是一个Python编写的开源网络爬虫框架。它是一个被设计用于爬取网络数据、提取结构性数据的框架。**

> Scrapy 使用了Twisted['twɪstɪd]异步网络框架，可以加快我们的下载速度。

> Scrapy文档地址：http://scrapy-chs.readthedocs.io/zh_CN/1.0/intro/overview.html

###  scrapy框架的作用

> 少量的代码，就能够快速的抓取

###  scrapy的工作流程

#### 回顾之前的爬虫流程

![](./scrapy/1.3.1.爬虫流程-1.png)

#### 上面的流程可以改写为

![](./scrapy/1.3.2.爬虫流程-2.png)

#### 3.3 scrapy的流程

![](./scrapy/1.3.3.scrapy工作流程.png)

##### 其流程可以描述如下：

1. 爬虫中起始的url构造成request对象-->爬虫中间件-->引擎-->调度器
2. 调度器把request-->引擎-->下载中间件--->下载器
3. 下载器发送请求，获取response响应---->下载中间件---->引擎--->爬虫中间件--->爬虫
4. 爬虫提取url地址，组装成request对象---->爬虫中间件--->引擎--->调度器，重复步骤2
5. 爬虫提取数据--->引擎--->管道处理和保存数据

##### 注意：

- 图中中文是为了方便理解后加上去的
- 图中绿色线条的表示数据的传递
- 注意图中中间件的位置，决定了其作用
- 注意其中引擎的位置，所有的模块之前相互独立，只和引擎进行交互

####  scrapy的三个内置对象

- request请求对象：由url method post_data headers等构成
- response响应对象：由url body status headers等构成
- item数据对象：本质是个字典

#### scrapy中每个模块的具体作用

##### ![](./scrapy/1.3.4 scrapy-组件.png)注意：

- 爬虫中间件和下载中间件只是运行逻辑的位置不同，作用是重复的：如替换UA等

## scrapy的入门使用

### 安装scrapy<br/>

命令:<br/>
&ensp;&ensp;&ensp;&ensp;sudo apt-get install scrapy<br/>
或者：<br/>
&ensp;&ensp;&ensp;&ensp;pip/pip3 install scrapy


### 2 scrapy项目开发流程

1. 创建项目:<br/>
   &ensp;&ensp;&ensp;&ensp;scrapy startproject mySpider
2. 生成一个爬虫:<br/>
   &ensp;&ensp;&ensp;&ensp;scrapy genspider xywx xywx.com
3. 提取数据:<br/>
   &ensp;&ensp;&ensp;&ensp;根据网站结构在spider中实现数据采集相关内容
4. 保存数据:<br/>
   &ensp;&ensp;&ensp;&ensp;使用pipeline进行数据后续处理和保存


### 3. 创建项目

> 通过命令将scrapy项目的的文件生成出来，后续步骤都是在项目文件中进行相关操作
>
> 创建scrapy项目的命令：<br/>
> &ensp;&ensp;&ensp;&ensp;scrapy startproject <项目名字><br/>
> 示例：<br/>
> &ensp;&ensp;&ensp;&ensp;scrapy startproject myspider

生成的目录和文件结果如下：<img src="./scrapy/2.1.scrapy入门使用-1.png" width = "60%" /> 

### 4. 创建爬虫

> 通过命令创建出爬虫文件，爬虫文件为主要的代码作业文件，通常一个网站的爬取动作都会在爬虫文件中进行编写。

命令：<br/>
&ensp;&ensp;&ensp;&ensp;**在项目路径下执行**:<br/>
&ensp;&ensp;&ensp;&ensp;scrapy genspider <爬虫名字> <允许爬取的域名><br/>

**爬虫名字**: 作为爬虫运行时的参数<br/>
**允许爬取的域名**: 为对于爬虫设置的爬取范围，设置之后用于过滤要爬取的url，如果爬取的url与允许的域不通则被过滤掉。<br/>

示例：

```shell
    cd myspider
    scrapy genspider xywy  xywy.com
```


### 5. 完善爬虫

#### 5.1 在/myspider/myspider/spiders/xywx.py中修改内容如下:

```
import scrapy

class xywxSpider(scrapy.Spider):  # 继承scrapy.spider
	# 爬虫名字 
    name = 'xywx' 
    # 允许爬取的范围
    allowed_domains = ['xywx.com'] 
    # 开始爬取的url地址
    start_urls = ['http://www.xywx.com/channel/teacher.shtml']
    
    # 数据提取的方法，接受下载中间件传过来的response
    def parse(self, response): 
    	# scrapy的response对象可以直接进行xpath
    	names = response.xpath('//div[@class="tea_con"]//li/div/h3/text()') 
    	print(names)

    	# 获取具体数据文本的方式如下
        # 分组
    	li_list = response.xpath('//div[@class="tea_con"]//li') 
        for li in li_list:
        	# 创建一个数据字典
            item = {}
            # 利用scrapy封装好的xpath选择器定位元素，并通过extract()或extract_first()来获取结果
            item['name'] = li.xpath('.//h3/text()').extract_first() # 老师的名字
            item['level'] = li.xpath('.//h4/text()').extract_first() # 老师的级别
            item['text'] = li.xpath('.//p/text()').extract_first() # 老师的介绍
            print(item)
```

##### 注意：

- scrapy.Spider爬虫类中必须有名为parse的解析
- 如果网站结构层次比较复杂，也可以自定义其他解析函数
- 在解析函数中提取的url地址如果要发送请求，则必须属于allowed_domains范围内，但是start_urls中的url地址不受这个限制，我们会在后续的课程中学习如何在解析函数中构造发送请求
- 启动爬虫的时候注意启动的位置，是在项目路径下启动
- parse()函数中使用yield返回数据，**注意：解析函数中的yield能够传递的对象只能是：BaseItem, Request, dict, None**

#### 5.2 定位元素以及提取数据、属性值的方法

> 解析并获取scrapy爬虫中的数据: 利用xpath规则字符串进行定位和提取

1. response.xpath方法的返回结果是一个类似list的类型，其中包含的是selector对象，操作和列表一样，但是有一些额外的方法
2. 额外方法extract()：返回一个包含有字符串的列表
3. 额外方法extract_first()：返回列表中的第一个字符串，列表为空没有返回None

#### 5.3 response响应对象的常用属性

- response.url：当前响应的url地址
- response.request.url：当前响应对应的请求的url地址
- response.headers：响应头
- response.requests.headers：当前响应的请求头
- response.body：响应体，也就是html代码，byte类型
- response.status：响应状态码



### 6 保存数据

> 利用管道pipeline来处理(保存)数据

#### 6.1 在pipelines.py文件中定义对数据的操作

1. 定义一个管道类<br/>
2. 重写管道类的process_item方法
3. process_item方法处理完item之后必须返回给引擎

```
import json

class xywxPipeline():
    # 爬虫文件中提取数据的方法每yield一次item，就会运行一次
    # 该方法为固定名称函数
    def process_item(self, item, spider):
        print(item)
        return item
```

#### 6.2 在settings.py配置启用管道

```
ITEM_PIPELINES = {
    'myspider.pipelines.xywxPipeline': 400
}
```

配置项中键为使用的管道类，管道类使用.进行分割，第一个为项目目录，第二个为文件，第三个为定义的管道类。<br/>
配置项中值为管道的使用顺序，设置的数值约小越优先执行，该值一般设置为1000以内。<br/>


### 7. 运行scrapy

命令：在项目目录下执行scrapy crawl <爬虫名字>

示例：scrapy crawl xywx