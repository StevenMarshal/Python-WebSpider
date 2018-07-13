## 2.3 使用Selenium爬取数据   

Selenium的作用就是通过操控浏览器来作一些事情，其最早被用于自动化测试、网络爬虫爬取数据、模拟人工登陆。

1：安装与配置Selenium  

Selenium的官网是：

    https://www.seleniumhq.org/

可以通过pip安装：

    C:\Users\Marshal>pip install selenium
    Collecting selenium
      Downloading https://files.pythonhosted.org/packages/41/c6/78a9a0d0150dbf43095c6f422fdf6f948e18453c5ebbf92384175b372ca2/selenium-3.13.0-py2.py3-none-any.whl (946kB)
    100% |████████████████████████████████| 952kB 13kB/s
    Installing collected packages: selenium
    Successfully installed selenium-3.13.0
    You are using pip version 9.0.1, however version 10.0.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.

    C:\Users\Marshal>

下载浏览器引擎GecKoDriver。  
下载网址：

    https://github.com/mozilla/geckodriver/releases

2：Selenium常用API  

Selenium操作浏览器主要是通过WebDriver对象实现，WebDriver对象提供了操作浏览器和访问HTML代码中数据的方法。  

操作浏览器方法如下：

* refresh()  

刷新网页。

* back()

回到上一个页面。

* forward()

进入下一个页面。

* close()

关闭窗口。

* quit()

结束浏览器执行。

* get(url)

浏览url所指的网页。

访问HTML代码中数据的方法：

* find_element_by_id(id_)

通过元素的id查找符合条件第一个元素。

* find_elements_by_id(id_)

通过元素的id查找符合条件的所有元素。

* find_element_by_name(name)

通过元素名字查找符合条件的第一个元素。

* find_elements_by_name(name)

通过元素名字查找符合条件的所有元素。

* find_element_by_link_text(link_text)

通过链接文本查找符合条件的第一个元素。

* find_elements_by_link_text(link_text)

通过链接文本查找符合条件的所有元素。

* find_element_by_tag_name(name)

通过标签名查找符合条件的第一个元素。

* find_elements_by_tag_name(name)

通过标签名查找符合条件的所有元素。

* find_element_by_xpath(xpath)

通过XPath查找符合条件的第一个元素。

* find_elements_by_xpath(xpath)

通过XPath查找符合条件的所有元素。

* find_element_by_class_name(name)

通过CSS中class属性查找符合条件的第一个元素。

* find_elements_by_class_name(name)

通过CSS中class属性查找符合条件的所有元素。

* find_element_by_css_selector(css_selector)

通过CSS中选择器查找符合条件的第一个元素。

* find_elements_by_css_selector(css_selector)

通过CSS中选择器查找符合条件的所有元素。


另外的一些常用的属性：

* current_url

获得当前页面的网址。

* page_source

返回当前页面的HTML代码。

* title

获得当前HTML页面的title属性值。

* text

返回标签中的文本内容。

例如：

    # coding=utf-8

    from selenium import webdriver

    driver = webdriver.Firefox()

    driver.get('http://q.stock.sohu.com/cn/600519/lshq.shtml')
    em = driver.find_element_by_id('BIZ_hq_historySearch')
    print(em.text)
    # driver.close()
    driver.quit()
