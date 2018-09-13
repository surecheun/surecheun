---
title: python手写bp神经网络实现人脸性别识别1.0
---

----------

写在前面：本实验用到的图片均来自google图片，侵删！


----------


### 实验介绍
用python手写一个简单bp神经网络，实现人脸的性别识别。由于本人的机器配置比较差，所以无法使用网上很红的人脸大数据数据集（如lfw数据集等等），所以我从google图片下载了一些中国明星的照片来作为本次实验的数据集。

- 训练数据集：5位中国的男明星（每个明星10张），6位中国的女明星（每个明星10张）。

- 测试数据集：6张女生，6张男生

### 实验环境

- win10

- python3.5+opencv+dlib+PIL

- 说明：上面涉及到的库都可以用pip install #### 轻易下载，但要注意它们之间的关联性，被依赖的库要先安装好。直接google就会有更详细的安装教程哦，所以这里不详说。。

### 实验步骤

#### 1 下载图片，构成数据集
我随机从google图片中下载了5位男明星的图片和6位女明星的图片。男明星的图片放在/photo/boys文件中，女明星的图片放在/photo/girls文件中。
然后，我又随机从google图片中下载了6张‘女生’的图片和6张‘男生’的图片，分别放在/girltest文件和/boytest文件中
男明星：
![男明星](https://img-blog.csdn.net/20180614160149565?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
女明星：
![女明星](https://img-blog.csdn.net/20180614160212991?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 2 框出人脸，并保存人脸区域
利用别人写好的人脸分类器来截取图片中的人脸，并把从训练集中截取到的人脸放到/faces中。图片排序为0.jpg,1.jpg...从女明星的图片开始读起，然后男明星的接上。具体函数对应get_face_from_photo（）函数。
人脸分类器下载：[人脸分类器](https://github.com/opencv/opencv/tree/master/data/haarcascades)

框住人脸：
![吴彦祖正脸](https://img-blog.csdn.net/2018061416031087?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![彭于晏侧脸](https://img-blog.csdn.net/2018061416035931?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![邓紫棋正脸](https://img-blog.csdn.net/20180614160421433?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

保存下来的人脸区域：
![人脸图集](https://img-blog.csdn.net/20180614160451783?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


#### 3 将人脸图片灰度化，且改为28*28大小
具体看函数change_photo_size28（）

灰度化后的部分图集：
![灰度化后的部分图集](https://img-blog.csdn.net/20180614160528225?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 4 训练
##### 4.1 读取图片的灰度值矩阵
读取图片的灰度值矩阵，读取出来的矩阵为（28，28）的，变为（784，1）的，然后把所有图片的的灰度值矩阵叠加成一个大的矩阵。
具体介绍参照：[参考1](https://blog.csdn.net/yunyunyx/article/details/80539222)、[参考2](https://blog.csdn.net/yunyunyx/article/details/80473532)
##### 4.2 训练、

- 梯度下降法

- sigmoid函数

具体介绍参照：[参考1](https://blog.csdn.net/yunyunyx/article/details/80473532)
[参考2](https://blog.csdn.net/yunyunyx/article/details/80539222)

#### 5 测试
测试图片的前期处理和训练图片的前期处理一样，先框出人脸，再灰度化和改变大小为28*28。
男生测试集：
![男测试集](https://img-blog.csdn.net/20180614160800666?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
女生测试集
![女测试集](https://img-blog.csdn.net/20180614160822304?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

规定预测出来的pre>0.5为男生，否则为女生。
### 完整代码

```
# -*- coding:utf-8 -*-
'''
内容：训练图片处理和人脸识别的训练部分
作者：surecheun
邮箱：surecheun@163.com
版本：1.0
'''
from PIL import Image
import sys
import dlib
import cv2
import os
import os.path
import numpy as np
import PIL.Image
from pylab import *

def get_face_from_photo(i,path,spath):
    '''
    利用别人写好的人脸分类器来截取图片中的人脸，并保存到spath中
    分类器下载：https://github.com/opencv/opencv/tree/master/data/haarcascades
    参考:官方文档
    '''
    detector = dlib.get_frontal_face_detector() #获取人脸分类
    # 读取path路径下的图片，获得所有的图片名字
    filenames = os.listdir(path)

    for f1 in filenames:
        f = os.path.join(path,f1)
        iimag = PIL.Image.open(f)
        # opencv 读取图片，并显示
        img = cv2.imread(f, cv2.IMREAD_COLOR)
 
        b, g, r = cv2.split(img)    # 分离三个颜色通道
        img2 = cv2.merge([r, g, b])   # 生成新图片

        counts = detector(img, 1) #人脸检测 
         
        for index, face in enumerate(counts):
            
            # 在图片中标注人脸，并显示
            left = face.left()
            top = face.top()
            right = face.right()
            bottom = face.bottom()
            
            #保存人脸区域
            j =str(i)
            j = j+'.jpg'
            save_path = os.path.join(spath,j)
            region = (left,top,right,bottom)
            #裁切图片
            cropImg = iimag.crop(region)

            #保存裁切后的图片
            cropImg.save(save_path)
            i +=1
            
            cv2.rectangle(img, (left, top), (right, bottom), (0, 255, 0), 3)
            cv2.namedWindow(f, cv2.WINDOW_AUTOSIZE)
            cv2.imshow(f, img)

    # 等待按键，退出，销毁窗口
    k = cv2.waitKey(0)
    cv2.destroyAllWindows()
    return i
    
    
def change_photo_size28(path,spath):
    '''
    将人脸图片转化为28*28的灰度图片
    '''
    
    filenames = os.listdir(path)

    for filename in filenames:
        f = os.path.join(path,filename)
        iimag = PIL.Image.open(f).convert('L').resize((28,28))
        savepath = os.path.join(spath,filename)
        #savepath = spath + '/' + filename
        iimag.save(savepath)
        

def read_photo_for_train(k,photo_path):
    '''
    读取训练图片
    '''
    for i in range(k):
        j = i
        j = str(j)
        st = '.jpg'
        j = j+st
        j = os.path.join(photo_path,j)
        im1 = array(Image.open(j).convert('L'))
        #（28，28）-->(28*28,1)
        im1 = im1.reshape((784,1))
        #把所有的图片灰度值放到一个矩阵中
        #一列代表一张图片的信息
        if i == 0:
            im = im1
        else:
            im = np.hstack((im,im1))
    return im
    

def layerout(w,b,x):

    '''
    sigmoid函数实现
    '''
    
    y = np.dot(w,x) + b
    t = -1.0*y
    # n = len(y)
    # for i in range(n):
        # y[i]=1.0/(1+exp(-y[i]))
    y = 1.0/(1+exp(t))
    return y

    
def mytrain(x_train,y_train):
    '''
    训练样本：中国某些明星的google图片(106张，女60张，男46张），侵删。女生标签为0，男生标签为1.
    训练方法：简单的梯度下降法
    参考（本人博客另一篇）：https://blog.csdn.net/yunyunyx/article/details/80539222
    '''
    
    '''
    设置一个隐藏层，784-->隐藏层神经元个数-->1
    '''

    step=int(input('mytrain迭代步数：')) 
    a=double(input('学习因子：')) 
    inn = 784  #输入神经元个数
    hid = int(input('隐藏层神经元个数：'))#隐藏层神经元个数
    out = 1  #输出层神经元个数

    w = np.random.randn(out,hid)
    w = np.mat(w)
    b = np.mat(np.random.randn(out,1)) 
    w_h = np.random.randn(hid,inn)
    w_h = np.mat(w_h)
    b_h = np.mat(np.random.randn(hid,1)) 

    for i in range(step):
        #打乱训练样本
        r=np.random.permutation(106)
        x_train = x_train[:,r]
        y_train = y_train[:,r]
        #mini_batch
        for j in range(100):
            x = np.mat(x_train[:,j]) 
            x = x.reshape((784,1))
            y = np.mat(y_train[:,j]) 
            y = y.reshape((1,1))
            hid_put = layerout(w_h,b_h,x) 
            out_put = layerout(w,b,hid_put) 

            #更新公式的实现
            o_update = np.multiply(np.multiply((y-out_put),out_put),(1-out_put)) 
            h_update = np.multiply(np.multiply(np.dot((w.T),np.mat(o_update)),hid_put),(1-hid_put)) 

            outw_update = a*np.dot(o_update,(hid_put.T)) 
            outb_update = a*o_update 
            hidw_update = a*np.dot(h_update,(x.T)) 
            hidb_update = a*h_update 

            w = w + outw_update 
            b = b+ outb_update 
            w_h = w_h +hidw_update 
            b_h =b_h +hidb_update 

    return w,b,w_h,b_h
    
def mytest(x_test,w,b,w_h,b_h):
    '''
    预测结果pre大于0.5，为男；预测结果小于或等于0.5为女
    '''
    hid = layerout(w_h,b_h,x_test);
    pre = layerout(w,b,hid);
    print(pre)
    if pre > 0.5:
        print("hello,boy!")
    else:
        print("hello,girl!")
        
        
        
#训练

#框出人脸，并保存到faces中,i为保存的名字
i = 0
#女孩
path = 'C:\\Users\\yxg\\Desktop\\photo\\girls'
spath = 'C:\\Users\\yxg\\Desktop\\faces'
i = get_face_from_photo(i,path,spath)
#男孩
path = 'C:\\Users\\yxg\\Desktop\\photo\\boys'
i = get_face_from_photo(i,path,spath)

#将人脸图片转化为28*28的灰度图片
path = 'C:\\Users\\yxg\\Desktop\\faces'
spath = 'C:\\Users\\yxg\\Desktop\\faces'
change_photo_size28(path,spath)

    
#获取图片信息
im = read_photo_for_train(106,spath)

#归一化
immin = im.min()
immax = im.max()
im = (im-immin)/(immax-immin)

x_train = im

#制作标签，前60张是女生，为0
y1 = np.zeros((1,60))
y2 = np.ones((1,46))
y_train = np.hstack((y1,y2))

#开始训练
print("----------------------开始训练-----------------------------------------")
w,b,w_h,b_h = mytrain(x_train,y_train)
print("-----------------------训练结束------------------------------------------")


#测试
print("--------------------测试女生-----------------------------------------")
#框出人脸，并保存到girltests中,i为保存的名字
i = 0
#女孩测试集
path = 'C:\\Users\\yxg\\Desktop\\girltest'
spath = 'C:\\Users\\yxg\\Desktop\\girltests'
i = get_face_from_photo(i,path,spath)

#将人脸图片转化为28*28的灰度图片
path = 'C:\\Users\\yxg\\Desktop\\girltests'
spath = 'C:\\Users\\yxg\\Desktop\\girltests'
change_photo_size28(path,spath)

    
#获取图片信息
im = read_photo_for_train(6,spath)

#归一化
immin = im.min()
immax = im.max()
im = (im-immin)/(immax-immin)

x_test = im
#print(x_test.shape)
for i in range(6):
    xx = x_test[:,i]
    xx = xx.reshape((784,1))
    mytest(xx,w,b,w_h,b_h)
print("---------------------测试男生-----------------------------")
#框出人脸，并保存到boytests中,i为保存的名字
i = 0
#男孩测试集
path = 'C:\\Users\\yxg\\Desktop\\boytest'
spath = 'C:\\Users\\yxg\\Desktop\\boytests'
i = get_face_from_photo(i,path,spath)
 
#将人脸图片转化为28*28的灰度图片
path = 'C:\\Users\\yxg\\Desktop\\boytests'
spath = 'C:\\Users\\yxg\\Desktop\\boytests'
change_photo_size28(path,spath)

    
#获取图片信息
im = read_photo_for_train(6,spath)

#归一化
immin = im.min()
immax = im.max()
im = (im-immin)/(immax-immin)

x_test = im
for i in range(6):
    xx = x_test[:,i]
    xx = xx.reshape((784,1))
    mytest(xx,w,b,w_h,b_h)

```

### 测试结果

```
----------------------开始训练--------------------------------------
mytrain迭代步数：300
学习因子：0.26
隐藏层神经元个数：28
-----------------------训练结束--------------------------------------
--------------------测试女生-----------------------------------------
[[0.00435441]]
hello,girl!
[[0.00160697]]
hello,girl!
[[0.47261838]]
hello,girl!
[[0.00344136]]
hello,girl!
[[0.00057052]]
hello,girl!
[[0.00030406]]
hello,girl!
---------------------测试男生-----------------------------
[[0.27352905]]
hello,girl!
[[0.63632333]]
hello,boy!
[[0.60296128]]
hello,boy!
[[0.68961767]]
hello,boy!
[[0.98755486]]
hello,boy!
[[0.99023972]]
hello,boy!
```

看结果，发现效果不错：6张女生图片都被识别对了，而男生只有一个被识别错误。。
