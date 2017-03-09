---
title: python小工具--遍历
date: 2016-09-28 18:36:45
tags:
---
The uses of this tool is about directory traversal, include varies of file type.As you know, the next step is to download these files, and email it to a direct address, that's wonderful.
工具作用为遍历电脑中指定目录中的所有文件及其路径，可以通过修改源码来指定所遍历的文件类型。
![image](upload_image/lu.jpg)
## Caffee first
### Traverse image
``` bash
#!/usr/bin/env python
#coding=utf-8

import os

def get_imlist(path):
	'return a list of file path'
	if os.path.isdir(path):
		files=os.listdir(path)
		for f in files:
			f=os.path.join(path,f)
			get_imlist(f)
	elif os.path.isfile(path):
		if path.endswith('.jpg'):
			print path
```


### sign:Bman
