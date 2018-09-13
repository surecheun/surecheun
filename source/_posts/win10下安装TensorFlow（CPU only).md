---
title: win10下安装TensorFlow（CPU only)
---

## TensorFlow安装过程
### 1 环境
我的安装环境：win10 + 64位 +miniconda2+miniconda创建的python3.5.5环境+pip

由于目前TensorFlow在windows下不支持python2.7的环境，而我机器原来的python版本就是miniconda2的2.7版本，所以一直无法安装TensorFlow，每次用pip安装，它都提示无法找到相应的版本。

所以，为了安装TesnsorFlow，我又利用miniconda创建了一个python3.5的环境。具体步骤如下。

1、打开安装miniconda时出现的程序“Anaconda Prompt”；
输入以下命令，先查看你已经安装了的环境；

```
conda env list
```
2、如果发现没有安装python3.5的话，就使用下面语句创建：

```
conda create --name your_env_name python=3.5
```
  其中your_env_name可以是你喜欢的任何名字，我的话就用了python3，因为是   python3.5嘛！
  
### 2 安装TensorFlow
按住win+R，打开cmd，进入命令窗口，先激活python3.5，输入下面命令：

```
activate your_env_name
```
如果你的命令行开头出现'(your_env_name)'，那么就是进入python3.5环境啦

然后直接用pip安装就可以啦:

```
pip install tensorflow
```
这里的安装需要一段时间，因为下载速度比较慢！！！

### 3 测试
等到成功安装后，在命令行输入python，进入python：

```
python
```
然后依次输入下面语句：

```
import tensorflow
a = tensorflow.constant(10)
b = tensorflow.constant(22)
sess = tensorflow.Session()
sess.run(a+b)
 
```
如果输出32，那就成功啦！
