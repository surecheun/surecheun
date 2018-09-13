---
title: 用python简便地抓取刘昊然的写真（17行代码）
---


#### 17行python代码抓取刘昊然图片之家的写真

用python来爬取网页信息是很简便的。因为它有很多库来帮助我们实现我们想要的功能。本实验用到的库有：requests和bs4中的BeautifulSoup。这两个库的安装过程如下：

```
#按住win+R，打开cmd，然后依次输入：
pip install bs4
pip install requests
```
在windows下爬取的话，还要检查自己是否安装了lxml。如果没安装，也可以直接用pip安装：

```
pip install lxml
```
安装好库之后呢，就可以开始爬取刘昊然的写真啦。

- 首先找到图片之家中刘昊然壁纸的网址：http://www.tupianzj.com/mingxing/xiezhen/liuhaoran/
由上面网址，我们可以翻译它的信息：http：//图片之家/明星/写真/刘昊然
所以，如果你要抓取其他的明星写真，只需要改变一下网址的最后一个就可以啦！

- 打开网址，右键，点击“检查”，然后你就可以看到这个网页的源代码啦。然后分析源代码，发现图片都存在下图的1中，而图片的存放格式都如下图的2、3那样：
![这里写图片描述](https://img-blog.csdn.net/20180425170616562?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- 找到规律后，我们就可以敲代码啦：
```python
#导入库
from bs4 import BeautifulSoup
import requests

#给定网址
URL = "http://www.tupianzj.com/mingxing/xiezhen/liuhaoran/"
```


```python
#抓取该URL的内容
html = requests.get(URL).text
#解析html，并存放在soup中
soup = BeautifulSoup(html, 'lxml')
#找到上面说的图中1的位置，因为图片都在它之中
img_ul = soup.find_all('div', {"id": "main"})
```

    


```python
#创建img文件夹来存放抓取到的图片
import os
os.makedirs('./img/',exist_ok=True)
```

```python
#由上图的2、3可知道图片的具体位置是在’img src‘中，所以先把所有的img找出来，再一一访问
imgs = ul.find_all('img')
#一一访问图片并下载
for img in imgs:
    url = img['src']
    r = requests.get(url, stream=True)
    image_name = url.split('/')[-1]
    with open('./img/%s' % image_name, 'wb') as f:
        for chunk in r.iter_content(chunk_size=128):#以128字节大小存放
            f.write(chunk)
    print('Saved %s' % image_name)
```

    Saved 9-1P31G623590-L.jpg
    Saved 9-1P3131419500-L.jpg
    Saved 9-1P3031414430-L.jpg
    Saved 9-1P3021543180-L.jpg
    Saved 9-1P3021123440-L.jpg
    Saved 9-1P22G043450-L.jpg
    Saved 9-1P1291JR50-L.jpg
    Saved 9-1P1221131480-L.jpg
    Saved 9-1P1051036070-L.jpg
    Saved 9-1P1051001240-L.jpg
    Saved 9-1G219115I70-L.jpg
    Saved 9-1G1151100100-L.jpg
    Saved 9-1G0301436130-L.jpg
    Saved 9-1G0041543170-L.png
    Saved 9-1FZ91523210-L.png
    Saved 9-1FHG13P60-L.png
    Saved 9-1F5201911020-L.jpg
    Saved 16-1612191430140-L.jpg
    Saved 16-160P11A0460-L.jpg
    Saved 9-16062G41001227.jpg
    Saved 16-1605301305090-L.jpg
    Saved 16-16051Q442070-L.jpg
    Saved 16-16051Q416050-L.jpg
    Saved 16-1605161012270-L.jpg
    Saved 16-1604131Z0240-L.jpg
    Saved 9-16012Q120410-L.jpg
    Saved 9-151224200S00-L.jpg
    
至此，图片抓取完啦，打开img文件夹看看：
![这里写图片描述](https://img-blog.csdn.net/20180425171802879?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
看，图片都下载到这啦。随便点开一张：
![这里写图片描述](https://img-blog.csdn.net/2018042517183811?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
帅帅的刘昊然！
