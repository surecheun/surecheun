---
title: opencv：vs2015添加了包含目录依然无法打开‘opencv2_core_core.hpp’ 解决方法
---

#### 安装环境

- win10

- vs2015

#### 出错和改错

按网上的教程，配置好opencv后，包括已经把以下内容添加到‘包含目录’了： 

```
E:\openCV\opencv\build\include
E:\openCV\opencv\build\include\opencv
E:\openCV\opencv\build\include\opencv2
```

输入测试代码：

```
#include <iostream>  
#include <opencv2/core/core.hpp>  
#include <opencv2/highgui/highgui.hpp>  

using namespace cv;

int main() {
	Mat img = imread("Aaron_Eckhart_0001.jpg");
	// 在窗口中显示avatar  
	imshow("Aaron_Eckhart_0001", img);
	// 等待6000 ms后窗口自动关闭    
	waitKey(6000);
}
```
执行调试，一直出现以下错误：无法打开‘opencv2/core/core.hpp’.......。Google了很久也找不到答案。。。失落了一会，突然发现#include opencv2/core/core.hpp 中opencv2已经在包含目录中了。。。。所以只要把路径中重复的opencv2去掉就好啦。因为opencv中很多的相关文件都包含了opencv2，所以，改一下刚刚添加到‘包含目录’中的第三项就好啦，改成：

```
E:\openCV\opencv\build\include
```

这样就可以啦！
