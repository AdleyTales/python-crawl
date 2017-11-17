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
                res.append({'m_title': t.find('.subject-img img').eq(i).attr('title'),'c_date': t.find('.main-meta').eq(i).text(),'m_rate_val': t.find('.main-title-rating').eq(i).attr('title')})
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
