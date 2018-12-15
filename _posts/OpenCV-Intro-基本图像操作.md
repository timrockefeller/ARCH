title: 'OpenCV Intro : 基本图像操作'
author: Kitekii
tags:
  - 'opencv '
  - coding
categories: []
date: 2018-10-20 11:37:00
---
Open Computer Vision 万物起源。
```python
import cv2
# OpenCV本体
import numpy as np
# 数学库
```

## 图像基本操作

### 读取、显示、保存
```python
IMG_PATH = "/path/to/img"
# 此处图片链接可能不支持中文
img = cv2.imread(IMG_PATH,cv2.IMREAD_COLOR)
```
opencv支持以下三种读入方式：
- cv2.IMREAD_**COLOR**: 默认，仅**(R,G,B)**通道
- cv2.IMREAD_**GRATSCALE**: 灰度
- cv2.IMREAD_**UNCHANGED**: 包括**(R,G,B,A)**通道

```python
cv2.imshow("WindowName",img)
# 创建一个名字为WindowName的窗口，把之前imread读入的照片以yuanchicun显示在窗口上。
cv2.waitKey()
# Press Any Key To Continue...
cv2.imwrite("/Path/to/CopiedPic",img)
```
<!--more-->
### 窗口创建与销毁
```python
cv2.namedWindow("WindowName0",cv2.WINDOW_AUTOSIZE)
# 根据图片大小自动调整
cv2.namedWindow("WindowName1",cv2.WINDOW_NORMAL)
# 窗口大小可调整
cv2.destroyAllWindows(.."WindowName") # 可不填参数
# 关闭全部(指定名称)窗口
```
### 宽高、通道数
`img.shape`返回图像**高**(图像矩阵的行数)、**宽**(图像矩阵的列数)、**通道数**三个属性组成的元组。
若图像是非彩色的，则只返回**高、宽**组成的元组。

```python
import cv2

img = cv2.imread("/path/to/img")
imgGrey = cv2.imread("/path/to/img",cv2.IMREAD_GRATSCALE)

sp1 = img.shape # (h,w,4)
sp2 = imgGrey.shape # (h,w)
```
图像总像素数和数据类型分别由`img.size`和`img.dtype`保存。一般情况下，图像的数据类型是uint8。

### 生成指定大小的空图像
使用numpy创建uint8格式的矩阵模型。
```python
imgZero = np.zeros(img.shape, np.uint8)
imgFix  = np.zeros((300,500,3), np.uint8)
# imgFix = np.zeros((300,500), np.uint8) # 灰度

cv2.imshow("imgZero",imgZero)
cv2.imshow("imgFix",imgFix)
```
### 像素获取、修改

可由坐标键值直接访问**(B,G,R)**元组。
```
numb = img[50,100]
img[50,100] = (0,0,255) # red
```
也能分开访问每一个通道。
```
img[0:100,100:200,0] = 255 # G
# 在原图片上覆盖了一个矩形
```
### 分离通道
```
b,g,r = cv2.split(img)
merged = cv2.merge([b,g,r])
```
### 添加文本
```
cv2.putText(img,                    # 图源
            text,                   # 文本
            org,                    # 起点坐标
            fontFace,               # 字体
            fontScale,              # 字体大小
            color,                  # 字体颜色
            thickness=None,         # 加粗
            lineType=None,
            bottomLeftOrigin=None)
```
### 缩放
```
imgg = cv2.resize(img, (300,100))
```
> 不知道拉伸的时候是失真还是像素重复