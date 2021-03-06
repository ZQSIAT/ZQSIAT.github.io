# 写在前面的：
经过一天的努力终于将数字图像处理的第一次实验课完成了，有点小激动，写一篇课程报告，在写之前特意咨询了胡老师，课程报告有没有固定格式？
得到的回答是否定的，不需要固定格式，心中不免有些欢喜，所以索性这篇课程报告成了我的第一篇DIP学习笔记。并且我会在后面附上我的源程序。
# 作业要求：
读图(Chapter2_1.pgm)、顺时针方向旋转15º（利用上述几种插值方法）、输出图像（测试图像见下图）,可用C++或Matlab编程。
要评估量化插值效果，并对结果进行讨论。
<br>提示：1.可正反转回到原点，然后比较未转的图像。
<br>2.如何定义评估量、其计算的合理性、原点的精确匹配等都要仔细考虑证实

<img width="150" height="150" src="/assets/img/Chaper2_1.jpg" />
<br>原图
<br>注意到了没有，这次作业的难点不在图像的旋转，而在于你如何评价图像旋转的好坏。

# 硬件平台：

因为上次采集数据集的需求，我的电脑在研究室算是比较好的了，请允许我再窃喜一次，嘿嘿。我使用的是一台
惠普Z440工作站，CPU是英特尔至强系列E5-1650，主频3.5GHz，内存16GB，硬盘256固态加2T机械磁盘阵列。

# 软件平台：

我使用win10系统、其中图像的旋转是在VS2015上使用openCV库，C++编程实现的，最后的性能评价是在
matlab2016a上进行分析的。

# 实验过程：

先说一下试验的总体过程，整个实验分为两部分，一部分使用openCV库函数实现，主要实现图片的2次旋转操作，
两次旋转都需要对图片的分辨率也就是大小进行操作，因为你要保证旋转是保留图像边沿，又要在旋转回原位时
保证和原图一样的大小，然后将原图和最终旋转后的图像存储为txt格式，方便后续的matlab读取和操作。
另一部分是用matlab编程实现对5种插值方法的比较，我主要使用了PSNR即“Peak Signal to Noise Ratio”的
缩写，即峰值信噪比进行比较两幅图像的相似度，还有图像矩阵的均方差，以及相关系数，这三种评价标准对5种
插值方法进行比较。

# 实验细节：

首先说一下第一部分，这部分比较困难，普及一下仿射变换，什么是仿射变换呢？
>百度百科: 仿射变换，又称仿射映射，是指在几何中，一个向量空间进行一次线性变换并接上一个平移，
>变换为另一个向量空间。仿射变换是在几何上定义为两个向量空间之间的一个仿射变换或者仿射映射
>（来自拉丁语，affine，“和…相关”）由一个非奇异的线性变换(运用一次函数进行的变换)接上一个平移变换组成。

在我的代码中我使用了openCV的仿射变换函数CV::warpAffine（）进行了仿射变换，它不是openCV中唯一一个仿射变换函数，
但是它可以设置插值方式，并且支持四边形仿射变换，功能很强大，我就决定了使用它了。在第一次旋转时你要首先将旋转后的
图像大小计算出来，然后生成一个临时图像，将原图像拷贝到临时图像的中心，再进行旋转，在可以避免缺失图像边角。
在第二次旋转时可以直接旋转，但是旋转后你要将原图像外围的图像扣掉，那就要你设置一个矩形区域，然后将原图抠出来。
最后就是每次试验设置一个参数就是插值方法，每次保存两个矩阵，原图像矩阵和旋转后的矩阵。其次补充一下matlab编程
这部分，为了得到很好的视觉效果，我花了很长时间在绘制一个直方图上。请看实验结果。嘿嘿。

# 实验结果及其分析：
试验结果分析，由图4可以看出，兰索思插值的效果最好，好于双三次插值，双三次插值好于双线性插值，
双线性插值好于最近邻插值，最近邻插值和基于区域的插值一样，是效果最差的插值方式。

<img width="150" height="150" src="/assets/img/Bilinear interpolation 1.jpg" />
<br>双线性插值第一次变换后的图像

<img width="150" height="150" src="/assets/img/Bilinear interpolation 2.jpg" />
<br>双线性插值第二次变换后的图像

# 代码
[C++代码链接](https://github.com/ZQSIAT/blog_code/blob/master/DIP%20Chapter2_1%20image%20rotation/image_rotation.cpp)
<br>[Matlab分析代码链接](https://github.com/ZQSIAT/blog_code/blob/master/DIP%20Chapter2_1%20image%20rotation/data%20analysis.m)


<br>[back to home page](./..)