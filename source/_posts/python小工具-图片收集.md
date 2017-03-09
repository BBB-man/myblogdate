---
title: python小工具--图片收集
date: 2016-09-30 16:34:37
tags:
---
It's the second step of my final instrument, this code is using to catch the picture in your computer and
copy it to a new fold. Waiting for my next step.

## A cup of tea:

### code:
``` bash
#!/usr/bin/env pthon
#coding=utf-8

import os, string, shutil
'These code is about create a new fold and copy the .jpg to this fold'

def mkdir(paths):

    paths=paths.strip()      # 去除首位空格
    # 去除尾部 \ 符号
    # paths=paths.rstrip("\\")
 
    isExists=os.path.exists(paths)  # 判断路径是否存在，存在（True）,不存在（False）

    if not isExists:
        # 如果不存在则创建目录
        print paths
        # 创建目录操作函数
        os.makedirs(paths)
        return True
    else:
        # 如果目录存在则不创建，并提示目录已存在
        print paths+' Already Created'
        return False
 
# 定义要创建的目录
mkpaths="e:\\12345678\\"
#调用函数生成目录
mkdir(mkpaths)


def get_imlist(path):
	'return a list of file path'
	if os.path.isdir(path):
		files=os.listdir(path)        #列出文件目录
		for f in files:
			f=os.path.join(path,f)	#关联文件和目录，形成路径
			get_imlist(f)
	elif os.path.isfile(path):
		if path.endswith('.jpg'):	#判断文件名为.jpg结尾的文件
			print path
			fname=os.path.basename(path)    #返回文件名
			new_f=mkpaths+fname				#在mkpath目录下创建相应文件
			shutil.copyfile(path,new_f)		#从path文件复制数据到new_f

path = raw_input("Enter path: ")
get_imlist(path)

```
![image2](upload_image/lu2.jpg)

### sign:bman