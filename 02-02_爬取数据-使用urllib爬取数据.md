## 2.2 使用urllib爬取数据

Urllib只能进行同步请求，不能进行异步请求。  
但是如果能找到获得动态数据的异步请求网址和参数，也可以通过urllib发送请求返回数据。  

1：获得静态数据  

    https://www.nasdaq.com/symbol/aapl/historical#.UWdnJBDMhHk

可以利用chrom浏览器，在右上角的打开菜单控制按钮下，找到“更多工具——开发者工具”来进行查看。

2：获得动态数据   

    http://q.stock.sohu.com/cn/600519/1shq.shtml

首先通过在页面中点击右键并选择“查看页面源代码”显示出源代码页面，在正常页面中复制关键股票价格数据，在源代码页面中进行查找，当在源代码页面中找不到时，可以判断为数据时经过动态获取的数据。  

通过在浏览器中点击“代开菜单”——“Web开发者”——“查看器”进入到页面信息查看功能。  

通过“网络”页签，这个网络就是指每次通信请求是异步请求还是同步请求。  

刷新的过程是重新发送请求：  
* HTML：指请求是静态的  
* XHR：指异步请求，就是我们说的Ajax请求  
* JS：也会有发送异步请求的可能  

有效请求的状态码为：200，表示请求成功了。  
通过“响应”页签观察从服务器返回的数据。通过从应答中找到感兴趣的数据。  

例如：  

    # coding=utf-8

    import re
    import urllib.request

    url = 'http://q.stock.sohu.com/hisHq?code=cn_600519&stat=1&order=D&period=d&callback=historySearchHandler&rt=jsonp&0.8115656498417958'

    req = urllib.request.Request(url)

    with urllib.request.urlopen(req) as response:
        data = response.read()
        json_data = data.decode('gbk')
        print(json_data)
        json_data = json_data.replace('historySearchHandler(', '')
        json_data = json_data.replace(')', '')
        print('替换后的：', json_data)

3：伪装成浏览器  

urllib的请求头中添加User-Agent字段
模拟iPhone浏览器：User-Agent设置用户代理字段，Mozilla/5.0(iPhone:CPU iPhone OS 10_2_1 like Mac OS X)  
AppleWebKit/602.4.6(KHTML, like Gecko) Version/10.0 Mobile/14D27 Safari/602.1说明这是iPhone的Safari浏览器发出的请求。

注解：User-Agent中文名为用户代理，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU类型、浏览器及版本，浏览器渲染引擎、浏览器语言、浏览器插件等。

示例：http://www.ctrip.com/

例如：

    import urllib.request

    url = 'http://www.ctrip.com/'

    req = urllib.request.Request(url)
    req.add_header('User-Agent',
               'Mozilla/5.0 (iPhone; CPU iPhone OS 10_2_1 like Mac OS X) AppleWebKit/602.4.6 (KHTML, like Gecko) Version/10.0 Mobile/14D27 Safari/602.1')

    with urllib.request.urlopen(req) as response:
        data = response.read()
        json_data = data.decode()
        if json_data.find('mobile') != -1:
            print('移动版')


    移动版
