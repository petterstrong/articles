# 图片Exif的Orientation参数说明



> 现代相机（包括手机的相机）大多都会内置方向传感器，会在按下快门（或者点击拍照）的时候记录下当前拍摄图片时相机的旋转方向，保证你查看图片时能自动旋转，或者方便后期编辑修改图片旋转方向。

在实际的开发中，图片旋转处理也是图片处理中比较重要的一个方面，直接影响用户良好的观感体验。

让我们一起看下图片的Exif中Orientation具体的参数含义:



## 一、参数

![示意图](http://opuln67cu.bkt.clouddn.com/image/orientation-1.001.jpeg)



> 以图片左上角为参考点，图片（景物）位置**不变**，相机以**左上角**为原点进行旋转

| 参数   | X1 对应相机的位置 | Y1对应相机的位置  | 旋转角度        |
| ---- | ---------- | ---------- | ----------- |
| 1    | Top        | Left-side  | 0°          |
| 2    | Top        | Right-side | 水平翻转（自拍）    |
| 3    | Bottom     | Right-side | 顺时针旋转180°   |
| 4    | Bottom     | Left-side  | 相机镜像倒置      |
| 5    | Left-side  | Top        | 顺时针90°+水平翻转 |
| 6    | Right-side | Top        | 顺时针90°      |
| 7    | Right-side | Bottom     | 顺时针90°+垂直翻转 |
| 8    | Left-side  | Bottom     | 逆时针90°      |

通过表格，我们可以看到orientation参数值在5-8之间的相机发生了旋转，X,Y轴之间互换，但是元数据的width和height并不会随旋转变化，也就是说，在5-8情况下的图片是需要我们根据进行手动的调换图片的width和height值的。



## 二、手机的处理



手机的相机也是横向的，所以，在竖屏的情况下拍摄出的图片均是需要处理的。



## 三、参考资料



[Exif 介绍](http://www.impulseadventure.com/photo/exif-orientation.html)
