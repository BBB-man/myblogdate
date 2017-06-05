---
title: python--爆破字典生成器
date: 2017-06-05 16:26:07
tags:
---
## 字典生成器
关于这个我很早就想写一个了，但是鉴于功力不够，有点敢做而不敢做的意思；今天终于腾空查了下资料，于是就有码了以下的代码：
``` bash
#!coding=utf-8

f=open('zidian.txt','w')
words = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcefghijklmnopqrstuvwxyz1234567890'
# 利用列表生成式可以进行多层循环生成list，下面的表达式是针对3位数密码，
#可视情况增加密码位数，还可以修改words内容来生成不同要求的密码
dic= [m+n+l for m in words for n in words for l in words]
for i in dic:
	print i
	f.write(i+'\n')
```

### 参考：
[参考地址1](http://blog.csdn.net/wangjianno2/article/details/51619468)
[参考地址2](http://www.sohu.com/a/109253500_218897)
[参考地址3](http://www.52pojie.cn/forum.php?mod=viewthread&tid=480259)

### 注意：
代码请勿用于非法用途，仅供本人学习参考，如有出现违法行为，与本人无关。