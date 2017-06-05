---
title: hexo博客异地同步
---
## hexo blog 异地同步
### 问题起源
很早前搭建了一个hexo的博客，用的是家里的电脑搭建的，一直只能在家里同步。于是和朋友讨论是否能在其它电脑上进行同步文件，很尴尬的一点是他成功了，而我却因为认证方式的问题一直未能进行异地同步。对于一个耿直的我来说分分钟想把这个博客撤了，内心真的是郁闷极了。看过网上有很多的类似的问题，也有和我报错相同的文章，但是都一一试过，结果还是以失败告终，真想爆一句“F**K”。通过分析发现，其实是一个登陆认证的问题；但是因为家里电脑用的是ssl登录认证，在deployer的时候并不需要输入密码。在git文件夹下翻了半天，结果都没有找到我的密码保存的目录。

### 环境搭建
首先，在新机子上搭建博客其实就是重复搭建hexo博客的过程，关键是新机子需要和你的github建立一个认证。
（一）准备工作：
前提：根据这篇文章[http://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/]
已经把家里pc上的内容put到github新项目中（https://github.com/BBB-man/myblogdate）
1.下载[node.js](https://nodejs.org/en/)，安装，默认会安装npm

2.安装[git](https://github.com/git-for-windows/git/releases)
注意：认证在此处出现
	关于github的配置，需要将新机器的SSH公钥添加到github上的https://github.com/settings/keys中,这里是关键。
输入以下命令，得到key：
``` bash
$ git config --global user.email "xxxxx@gmail.com"    #enter your email(github registered)
$ git config --global user.name "xxxx"		#enter whatever
```
回车到底，此时会默认在c盘中（C:\Users\xxx\.ssh\）目录下生成文件id_rsa/id_rsa.pub。
![id_rsa.pub](/upload_image/id_rsa_pub.jpg)
然后将id_rsa.pub文件中的内容复制到github的key文本框中。
![gitkey](/upload_image/gitkey.jpg)
可以通过命令：
``` bash
$ ssh -T git@github.com
```
来验证一下

3.安装hexo    打开cmd, 运行：npm install -g hexo

*创建一个Myblog文件夹
*把github中的备份文件clone到Myblog中
``` bash
$ git clone https://github.com/BBB-man/myblogdate
```
*在Myblog中运行命令：(会产生一个hexo列表)
``` bash
$ hexo init
```
*将Myblog里面的node_modules和scafflods文件复制到clone下来的myblogdate文件夹中

（二）问题调试
1. 修改本地_config.yml文件的deploy配置
``` bash
deploy:
type: git
repository: git@github.com:BBB-man/BBB-man.github.io.git   #这里改成这个，原先为https的
branch: master
```
2. 做完上述的工作，说明认证方式已经没问题了。但是因为某些原因在执行“hexo d”的操作中会报错，原因我就不多说了，直接执行以下命令：
``` bash
$ npm install hexo-deployer-git --save
```
3. 如果出现无法在c盘users/xxx/.ssh/生成ssh密钥的情况，那么使用如下命令：
``` bash
$ ssh-keygen -t rsa -C "你的邮箱地址"
```
### sign: bman