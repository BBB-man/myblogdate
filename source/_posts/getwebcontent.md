---
title: GetWebContent
date: 2016-11-21 14:19:19
tags:
---
代码的作用是抓取页面“http://top.chinaz.com/diqu/index_ZheJiang_2.html-100.html”页面中列出来的浙江网站的名称+url；
但是过程中遇见了几个难点：正则匹配、url的分解重组、异常处理以及最后保存到excel中内容被覆盖的问题等。
1. 在使用正则的过程中，难点是正则规则的写法。比如：怎么使用"非贪婪"模式，怎么进行循环遍历。
2. 在url重组的时候，要使用type()查看类型，保证+号前后都是字符串，或整型。
3. 异常处理语句：(try------except语句)。
4. excel保存内容，先把获取到的数据保存到列表当中，最后把整个列表一次性保存到excel中，这样就可以保证内容不被覆盖。
![Alt text](\upload_image\getweb2.jpg)
## 代码块：
### python--re模块
	这个模块提供了与 Perl 相似l的正则表达式匹配操作。Unicode字符串也同样适用
	正则表达式使用反斜杠" \ "来代表特殊形式或用作转义字符，这里跟Python的语法冲突，因此，Python用" \\\\ "表示正则表达式中的" \ "，因为正则表达式中如果要匹配" \ "，需要用\来转义，变成" \\ "，而Python语法中又需要对字符串中每一个\进行转义，所以就变成了" \\\\ "	
	为了使正则表达式具有更好的可读性，Python特别设计了原始字符串(raw string)，需要提醒你的是，在写文件路径的时候就不要使用raw string了，这里存在陷阱。raw string就是用'r'作为字符串的前缀，如 r"\n"：表示两个字符"\"和"n"，而不是换行符了。Python中写正则表达式时推荐使用这种形式
	绝大多数正则表达式操作与模块级函数或RegexObject方法一样都能达到同样的目的。而且不需要你一开始就编译正则表达式对象，但是不能使用一些实用的微调参数
more info: [re模块简介](http://www.cnblogs.com/PythonHome/archive/2011/11/19/2255459.html)

### python--xlwt/xlrd
python中的xlwt是创建一个excel，写入新的内容。
python中的xlrd是修改已存在的excel中的内容。
more info: [xlwt/xlrd简介](http://www.cnblogs.com/MrLJC/p/3715783.html)


![Alt text](\upload_image\getweb.jpg)
``` bash
#coding=utf-8

import urllib2
import re
import xlwt

num = 2
workbook=xlwt.Workbook()		#创建一个workbook对象
sheet1=workbook.add_sheet('sheet1',cell_overwrite_ok=True)	#创建一个sheet对象
name_list=[]
url_list=[]
while(num<=100):
	userMainUrl = "http://top.chinaz.com/diqu/index_ZheJiang_" + str(num) + ".html"		#对url进行拆分，进行循环
	req = urllib2.Request(userMainUrl)
	resp = urllib2.urlopen(req)
	respHtml = resp.read()			#把网页源代码提取出来赋值给respHtml
	num = num+1
	#print "respHtml=", respHtml     

	#class="pr10 fz14">三国杀官方网站</a>    需要匹配的名字
	#</a><span class="col-gray">sanguosha.com</span>   需要匹配的url
	Url = '</a><span\s+?class="col-gray">(?P<gray>.+?)</span>'
	Name = '\s+?class="pr10 fz14">(?P<pr10>.+?)</a>'	#正则表达式，表示匹配的规则"\s+?、(?P<>.+?)"
	def find(value):
		comp1 = re.compile(value)			 #将正则表达式编译，提高运行速度。
		foundNm = comp1.findall(respHtml)	#表示在respHtml中进行匹配
		return foundNm         #findall会返回一个列表，列表中的对象是字符串或乱码。
		# for message in foundNm:				#用for循环把列表的内容读取出来
			# print message.decode('utf-8')    #.decode('utf-8')

	lsn = find(Name)
	lsu = find(Url)

	name_list+=lsn			#先把获取到的数据保存到列表当中，最后把整个列表一次性保存到excel中
	url_list+=lsu

count=len(name_list)
i=0
while(i < count):
	n=name_list[i]
	u=url_list[i]
	i+=1
	try:
		print n.decode("utf-8"), u
	except UnicodeEncodeError:			#try...except语句抓取报错，避免出现一个错误导致后面的都不执行了。
		print n, u
	sheet1.write(i,0,n.decode("utf-8"))
	sheet1.write(i,1,u)
else:
	print 'loading......'

workbook.save('xxx.xls')

print 'all the pages were done!!!'
```

### sign: bman