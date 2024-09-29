---
layout:             post
title:              困难爬虫分析一例
subtitle:           爬取NHC公开疫情数据
date:               2023-01-21
author:             Tresol
original:           True
original_author:    Tresol
header-style:       text
catalog:            true
tags:
    - Python
    - 计算机语言
    - 爬虫
---

### 正文

国家卫生健康委员会（National Health Commission，NHC）的网站十分难爬，Tresol用`urllib`爬的时候遇到了许多错误（包括但不限于`HTTP:Error412`、`SSL:Error997`）。

于是，Tresol想到了一个专门用来反反爬虫的库——`pypprteer`。实现原理如下：

#### 分析网站

##### 网址

NHC的网站是：[http://www.nhc.gov.cn/](http://www.nhc.gov.cn)。所需要爬取的疫情日报页面是：[http://www.nhc.gov.cn/xcs/yqtb/list_gzbd.shtml](http://www.nhc.gov.cn/xcs/yqtb/list_gzbd.shtml)。

点击连续三个日报，观察其网址分别为：
```
http://www.nhc.gov.cn/xcs/yqtb/202212/cb666dbd11864171b6586887c964791c.shtml
http://www.nhc.gov.cn/xcs/yqtb/202212/77225a2e37e4438fb888304078372a82.shtml
http://www.nhc.gov.cn/xcs/yqtb/202212/77225a2e37e4438fb888304078372a82.shtml
```
显得毫无规律。

但是，如果我们翻阅三个目录页的网址：
```
http://www.nhc.gov.cn/xcs/yqtb/list_gzbd.shtml
http://www.nhc.gov.cn/xcs/yqtb/list_gzbd_2.shtml
http://www.nhc.gov.cn/xcs/yqtb/list_gzbd_6.shtml
```

发现：除了第一页没有标号外，其他几页都是按照`'http://www.nhc.gov.cn/xcs/yqtb/list_gzbd_'+<Page>+'.shtml'`构成的。这就为爬取数据提供了便利。

因此封装函数：
```python
def getPageUrl():
    for i in range(1,47):
        if i==1:
            yield 'http://www.nhc.gov.cn/xcs/yqtb/list_gzbd.shtml'
        else:
            url='http://www.nhc.gov.cn/xcs/yqtb/list_gzbd_'+str(i)+'.shtml'
            yield url
```

#### 网页结构

随意打开一个目录页的源代码，发现其链接是以以下形式展现的：
```html
<li>
	<a href="/xcs/yqtb/202208/8802f7342e2c4bd7ac2d123930f3aedc.shtml" target="_blank" title="截至*月*日24时新型冠状病毒肺炎疫情最新情况">截至*月*日24时新型冠状病毒肺炎疫情最新情况</a>
	<span class="ml">202*-*-*</span>
</li>
```
所有的文章，都存放在一个` class `为` zxxx_list `的 ul 标签下，其中每一个` li `标签对应着一篇文章。

打开疫情日报网页，截取一段源代码如下：
```html
<div class="con" id="xw_box">
	<p style="text-align: justify; line-height: 1.5; text-indent: 2em; font-family: 仿宋,仿宋_GB2312; font-size: 16pt; -ms-text-justify: inter-ideograph;">
		<span style="font-family: 仿宋,仿宋_GB2312; font-size: 16pt;">*******************************************</span>
		<span style="font-family: 仿宋,仿宋_GB2312; font-size: 16pt;"><o:p></o:p></span></p>
</div>
```
可见，疫情日报正文即存放于标签为`xw_box`的文本框中。我们只要抓取`div`中的文本即可。

### 实战

#### 导入必要的库


```python
# Import necessary repoes.
import asyncio
import os
from pyppeteer import launcher
launcher.DEFAULT_ARGS.remove('--enable-automation')
from pyppeteer import launch
from bs4 import BeautifulSoup
```

未安装`pyppeteer`、`asyncio`、`bs4`等库的可以用cmd指令`pip install <Name>`命令来实现库的安装。同时，为防止意外，请自备ladder。

其中，`launcher.DEFAULT_ARGS.remove('--enable-automation')`的作用是防止服务端监测`webdriver`。

#### 封装函数

1. 封装`pyppteer`。

```python
# Define necessary functions.
async def pyppteer_fetchUrl(url):
    browser = await launch({'headless': False,'dumpio':True, 'autoClose':True})
    page = await browser.newPage()

    await page.goto(url)
    await asyncio.wait([page.waitForNavigation()])
    str = await page.content()
    await browser.close()
    return str

def fetchUrl(url):
    return asyncio.get_event_loop().run_until_complete(pyppteer_fetchUrl(url))
```

2. 获取标题、链接与日期。

```python
def getTitleUrl(html):
    bsobj=BeautifulSoup(html,'html.parser')
    titleList = bsobj.find('div', attrs={"class":"list"}).ul.find_all("li")
    for item in titleList:
        link = "http://www.nhc.gov.cn" + item.a["href"];
        title = item.a["title"]
        date = item.span.text
        yield title, link, date
```

3. 获取正文内容。

```python
def getContent(html):
        
    bsobj = BeautifulSoup(html,'html.parser')
    cnt = bsobj.find('div', attrs={"id":"xw_box"}).find_all("p")
    s = ""
    if cnt:
        for item in cnt:
            s += item.text
        return s
    return "爬取失败！"
```

4. 保存文件。(将文件保存至本地的文件夹中)

```python
def saveFile(path, filename, content):

    if not os.path.exists(path):
        os.makedirs(path)
    with open(path + filename + ".txt", 'w', encoding='utf-8') as f: # Save
        f.write(content)
````
#### 主函数

```python
if "__main__" == __name__: 
    for url in getPageUrl(): # 生成网址
        s =fetchUrl(url)
        for title,link,date in getTitleUrl(s): # 解析列表中的文章
            print(title,link)
            year=int(date.split("-")[0])
            html =fetchUrl(link)
            content = getContent(html)
            print(content)
            saveFile("D:/Python/NHC_Data/", str(year)+title, content)
            print("-----"*10)
```
完成！

### 结语

爬虫虽然可以快速获取数据，但是，这种方式如果使用不当很容易造成服务器资源的不必要浪费。因此，很多网站都在不断强化反爬功能（NHC便是一个典型例子）。

因此，上文所述的爬虫可能在短时间内失效。

2023年1月21日是中国传统农历新年，祝各位春节快乐！
