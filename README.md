# python爬虫

> 爬取豆瓣电影

### 准备
- python3
- requests
- pyquery

### 安装包

    pip3 install requests pyquery

### 爬取电影评论 时间 推荐指数

```python

    # -*- coding: utf-8 -*-

    import requests
    from pyquery import PyQuery as pq


    # page 20/page urls
    def get_url(total):
        urls = []
        url_num = list(range(0,total,20))

        # base url
        url = 'https://movie.douban.com/review/best/?start='

        for x in url_num:
          urls.append(url+str(x))

        return urls

    # result
    def get_res(total):
        urls = get_url(total)
        res = []

        for url in urls:
            r = requests.get(url)
            t = pq(r.text)
            len = t.find('.subject-img img').length
            i = 0
            while i < len:
                res.append({'m_title': t.find('.subject-img img').eq(i).attr('title'),'c_date':
                
                 t.find('.main-meta').eq(i).text(),'m_rate_val': t.find('.main-title-rating')
                 .eq(i).attr('title')})

                 i = i+1

        return res

    # ceshi because DOUBAN total 50 data
    result = get_res(301)
    print(len(result)) #50
    print(result)

    print('=============================')
    print('=============================')
    print('=============================')

    # 分析出最好的电影影评 5分 力荐

    data = []
    for rate in result:
        if rate['m_rate_val'] == "力荐":
            data.append(rate)

    print(data)



```



---
---
---


# 爬虫

### 爬虫分类：

**1.通用爬虫：** 搜索引擎用的爬虫系统

    搜索引擎如何获取到url进行爬取?
      1.主动向向搜索引擎提交网址：登陆百度站长平台，提交您的网站  维护新的站点 提交到百度
      2.在其他网站里设置网站的外链
      3.搜索引擎会和DNS服务商进行合作，可以快速收录新的网站

    DNS：就是把域名解析成IP地址的技术

    通用爬虫并不是万物都是可爬取的：
        遵循 Robots 协议：知名通用爬虫可以爬取网页的权限
        Robots.txt 只是一个建议，并不是所有爬虫都遵循，一般只有大型的所有爬虫
        个人爬虫不要管这些

    通用爬虫工作流程： 爬取网页  存储数据 内容处理 提供检索/排名服务

    搜索引擎排名：
      1.pageRank值：根据网站的流量（点击量/浏览量/人气）,流量越高，排名靠前，值钱。
      2.竞价排名： 谁给钱多，谁排名就高。

    通用爬虫缺点：
      1.只能根据和文本相关的内容 html world pdf，但不能提供音乐 图片 视频，二进制文件（脚本）
      2.提供的结果前篇一律，不能针对不同背景领域的人提供不同的搜索结果
      3.不能理解人类语义上的检索

**2.聚焦爬虫:** 爬虫程序员写的针对某种内容的爬虫。面向主题爬虫，面向需求爬虫，会针对某种特定的内容爬取信息，会保证信息相关。

### HTTP和HTTPS协议：

https协议就是http的安全版，在http下加入SSL层。（安全套阶层），主要用于web的安全传输协议。
http 的端口默认：80
https 的端口默认是：443

爬虫的原理就是模拟浏览器的原理

客户端  服务器

发送请求 request

抓包工具：Telerik Fiddker Web Debugger

cookie 模拟登陆

### urllib库

pyton3 中改为urllib.request
