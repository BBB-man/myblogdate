---
title: python--shodan
date: 2017-03-24 15:13:58
tags:
---
百度一下shodan：black-google，这足以表达出shodan的强大，于是乎就想研究一下shodan的搜索功能。没想到python已经有这个模块了https://shodan.readthedocs.io/en/latest/tutorial.html#connect-to-the-api，但是上面都是英文，看着不太舒服，索性就顺手翻译一下吧。
### 第一步：安装
在开始在python使用shodan库之前呢，首先确保你已经注册shodan并且拿到了API key。然后才能安装并使用(这里使用pip安装)：
``` bash
>>>python
>>>pip install shodan
```
### 连接API
在调用接口前必须先使用shodan的API连接
``` bash
import shodan
SHODAN_API_KEY = "insert your API key here"

api = shodan.Shodan(SHODAN_API_KEY)
```
### shodan搜索
现在我们已经完成前期配置，可以使用shodan提供的API进行初步搜索：
``` bash
#利用try/ except 模块抓取错误，直接跳过，不影响运行
try:
    #serach shodan
    results = api.search('Apache')        # 输入搜索信息

    #打印搜索结果
    print 'Results found: %s' % results['total']
    for result in results['matches']:
        print 'IP: %s' % result['ip_str']
        print result['data']
        print ''
except shodan.APIError, e:
    print 'Error: %s' % e
```
通过代码，首先我们利用api对象的一个Shodan.search()方法给我们返回一个包含结果的字典信息；这里普通的shodan用户只能返回前100个结果。

同时，利用该对象函数还能返回更多的信息，下面是Shodan.search()返回的部分结果：
``` bash
{
        'total': 8669969,
        'matches': [
                {
                        'data': 'HTTP/1.0 200 OK\r\nDate: Mon, 08 Nov 2010 05:09:59 GMT\r\nSer...',
                        'hostnames': ['pl4t1n.de'],
                        'ip': 3579573318,
                        'ip_str': '89.110.147.239',
                        'os': 'FreeBSD 4.4',
                        'port': 80,
                        'timestamp': '2014-01-15T05:49:56.283713'
                },
                ...
        ]
}
```
返回更多属性方法列表：[REST API documentation](https://developer.shodan.io/api)
### host查询
我们可以用shodan.host()方法来查询ip等内容信息：
``` bash
# Lookup the host
host = api.host('217.140.75.46')        #输入ip域名

# Print general info
print """
        IP: %s
        Organization: %s
        Operating System: %s
""" % (host['ip_str'], host.get('org', 'n/a'), host.get('os', 'n/a'))

# Print all banners
for item in host['data']:
        print """
                Port: %s
                Banner: %s

        """ % (item['port'], item['data'])
```
下面我罗列了几种(/shodan/host/{ip})   host.get()内部的方法属性
{
region_code,ip,area_code,country_name,postal_code,dma_code,
country_code,data,os,product,timestamp,asn,banner...   
}

## 总结
首先利用python脚本来调用shodan的API还是非常方便的；在笔者看来，最麻烦的无非是确定API接口的各种属性方法。
例如：
``` bash
msg = api.host('xxx.xxx.xxx.xxx')

print """
        msg1: %s
        msg2: %s
        msg3: %s
        msg4: %s
        msg5: %s
""" % (host.get('region_code', 'n/a'),host.get('area_code','n/a'),host.get('asn', 'n/a'),host.get('product', 'n/a'),)

```
总之，利用这些属性可以查看各种有用的信息，你懂得！！！

sign:B-man
