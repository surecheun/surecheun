---
title: python将图片转化为字符图
---

﻿最近看到将图片转化为字符图的小实验，我觉得很有趣，所以决定自己实现一下。

步骤和原理如下：

- 读取图片的灰度值矩阵（0-255之间），灰度值矩阵主要反映的是图片的黑白程度，越黑越接近与0，越白越接近于255

- 确定用于作画的字符

- 根据灰度值确定代替字符，灰度值越小，其代替字符应该笔画越多（这样才能看起来颜色更深）

- 把全部选好的代替字符写入文本

- 选择字符的做法：用256（0-255又256个数）除以可以用于作画的字符的总长度，然后得到一个字符的灰度值区间。然后灰度值在某个区间是就转化为指定的字符。

- 我从google图片下载了一张小猪佩奇的图片，侵删，然后用它来画字符画，结果如下：
![小猪佩奇](https://img-blog.csdn.net/20180627110233954?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![小猪佩奇字符画](https://img-blog.csdn.net/20180627110400463?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

还是蛮像的!
代码如下：

```
# coding: utf-8

import numpy as np
from PIL import Image

def img_to_char(image_path,height):
    '''
    将图片转化为字符
    image_path是图片的路径
    height是字符串图片的高度
    '''
    
    #读取图片
    img = Image.open(image_file)
    img_width, img_height = img.size
    # 假设字符的宽度是高度的3倍
    width = 3* height * img_width // img_height
    img = img.resize((width, height), Image.ANTIALIAS)
    #读取图片的灰度值矩阵
    data = np.array(img.convert('L'))
    #设定字符,字符数要是256的因子，这里取32
    chars = "#RMNHQODBWGPZ*@$C&98?32I1>!:-;. "
    N = len(chars)
    #计算每个字符的区间,//取整
    n = 256 // N
    #result是字符结果
    result = ''
    for i in range(height):
        for j in range(width):
            result += chars[data[i][j] // n]
        result += '\n'
    with open('img.txt', mode='w') as f:
        f.write(result)


if __name__ == '__main__':
    image_file = '10.jpg'
    height = 100
    img_to_char(image_file,height)
 
```


