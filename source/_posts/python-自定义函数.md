---
title: python--自定义函数
date: 2016-09-27 14:17:23
tags:
---
python的自定义函数创建方法：首先把功能写好，然后直接使用函数语法，调用函数即可。关键一步是在函数的功能上。

## Quick Start

### python函数的语法：
``` bash
def function_name(arguments):
	"function_documentation_string"     #函数声明
	function_body_suite		#定义体
```

### 写好的功能语句：
``` bash
'rtf -- read text filename'
fname=raw_input('Enter filname: ')
print 'we are going to... '
# attempt to open file for reading
try:
	fobj=open(fname, 'r')
except IOError, e:
	print "*** file open error:", e
else:
	for eachline in fobj:
		str=eachline
		print str.strip('')
	fobj.close()
```

### 改成函数rtf():
``` bash
def rtf():
	'rtf -- read text filename'
	fname=raw_input('Enter filname: ')
	print 'we are going to... '
	# attempt to open file for reading
	try:
		fobj=open(fname, 'r')
	except IOError, e:
		print "*** file open error:", e
	else:
		for eachline in fobj:
			str=eachline
			print str.strip('')
		fobj.close()
rtf()
```

### sign:bman