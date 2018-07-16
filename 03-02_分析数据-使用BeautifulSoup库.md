## 3.2 使用BeautifulSoup库

BeautifulSoup是一套帮助程序设计师解析网页结构项目。  
BeautifulSoup官网是：  

    https://www.crummy.com/software/BeautifulSoup/。

1：安装BeautifulSoup

    pip install beautifulSoup4

2：BeautifulSoup常用API

BeautifulSoup中主要使用的对象是BeautifulSoup实例，BeautifulSoup常用方法如下：  

* find_all(tagname)

根据标签名返回所有符合条件的元素列表。

* find(tagname)

根据标签名返回符合条件的第一个元素。

* select(selector)

通过CSS中选择器查找符合条件所有元素。

* get(key, default = None)

获取标签属性，key是标签属性名。

BeautifulSoup常用属性如下：  

* title

获得当前HTML页面的title属性值。

* text

返回标签中的文本内容。

BeautifulSoup解析器，可以使用的解析器有4个：

* html.parser

Python编写的解析器，速度比较快，支持Python 2.7.3和Python 

### 3.2.2 以上版本。

* lxml

C编写的解析器，速度很快，依赖于C库。如果是CPython环境可以使用该解析器。

* lxml-xml

C编写的XML解析器，速度很快，依赖于C库。

* html5lib

HTML5解析器。

例如：

    # coding=utf-8

    import os
    import urllib.request

    from bs4 import BeautifulSoup

    url = 'http://p.weather.com.cn/'


    def findallimageurl(htmlstr):

        sp = BeautifulSoup(htmlstr, 'html.parser') #html.parser lxml
        # 返回所有的img标签对象
        imgtaglist = sp.find_all('img')

        # 从img标签对象列表中返回对应的src列表
        srclist = list(map(lambda u: u.get('src'), imgtaglist))
        # 过滤掉非.png和.jpg结尾文件src字符串
        filtered_srclist = filter(lambda u: u.lower().endswith('.png')
                                        or u.lower().endswith('.jpg'), srclist)

        return filtered_srclist

    def getfilename(urlstr):

        pos = urlstr.rfind('/')
        return urlstr[pos + 1:]


    # 分析获得的url列表
    url_list = []
    req = urllib.request.Request(url)
    with urllib.request.urlopen(req) as response:
        data = response.read()
        json_data = data.decode()

        url_list = findallimageurl(json_data)

    for imagesrc in url_list:
        # 根据图片地址下载
        req = urllib.request.Request(imagesrc)
        with urllib.request.urlopen(req) as response:
            data = response.read()
            # 过滤掉用小于100kb字节的图片
            if len(data) < 1024 * 100:
                continue

            # 创建download文件夹
            if not os.path.exists('download'):
                os.mkdir('download')

            # 获得图片文件名
            filename = getfilename(imagesrc)
            filename = 'download/' + filename
            # 保存图片到本地
            with open(filename, 'wb') as f:
                f.write(data)

        print('下载图片', filename)
