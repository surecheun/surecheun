---
title: 用python的turtle画分形树
---

﻿由于分形树具有对称性，自相似性，所以我们可以用递归来完成绘制。只要确定开始树枝长、每层树枝的减短长度和树枝分叉的角度，我们就可以把分形树画出来啦！！
代码如下：

```
# -*- coding: utf-8 -*-
'''
绘制分形树
'''

import turtle as tl 

def draw_smalltree(tree_length,tree_angle):
	'''
	绘制分形树函数
	'''
	if tree_length >= 3:
		tl.forward(tree_length) #往前画
		tl.right(tree_angle)  #往右转
		draw_smalltree(tree_length - 10,tree_angle)#画下一枝，直到画到树枝长小于3

		tl.left(2 * tree_angle)  #转向画左
		draw_smalltree(tree_length -10,tree_angle) #直到画到树枝长小于3

		tl.rt(tree_angle) #转到正向上的方向，然后回溯到上一层
		if tree_length <= 30:  #树枝长小于30，可以当作树叶了，树叶部分为绿色
			tl.pencolor('green')
		if tree_length > 30:
			tl.pencolor('brown')  #树干部分为棕色
		tl.backward(tree_length)  #往回画，回溯到上一层



def main():
	tl.penup()
	#tl.pencolor('green')
	tl.left(90)  #因为树是往上的，所以先把方向转左
	tl.backward(250) #把起点放到底部
	tl.pendown()
	tree_length = 100  #我设置的最长树干为100
	tree_angle = 20  #树枝分叉角度，我设为20
	draw_smalltree(tree_length,tree_angle)
	tl.exitonclick()  #点击才关闭画画窗口

if __name__ == '__main__':
	main()

```
结果如下：
![分形树](https://img-blog.csdn.net/20180416145713941?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bnl1bnl4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
