- 线性回归函数（20222.09.12）

  > 自定义一些数据，把它们和标准数据混合在一起，再用线性回归方式去提取出来：
  
  > ![](/run/user/1000/doc/1e7a0e02/2022-09-12 21-17-41 的屏幕截图.png)
  >
  > - normal 是正态分布，即：x是随机数据，y是……（我也不知道是什么了，理论基础不够就来搞实践太痛苦了）



- cv::Mat::resize 和cv::resize(2022.9.17)

  >前者是resize(size_t row,(scale)) ,通过行来确定图像，后者是全确定图像resize(width,high);

- opencv 中的常用滤波器：https://streamja.com/RW7wZ

  > - 均值滤波器：void blur( InputArray src, OutputArray dst, Size ksize, Point anchor = Point(-1,-1),int borderType = BORDER_DEFAULT );
  >
  >   > 卷积核是全为一的ksize大小的矩阵，与原图形卷积得到输出图像；边界处理，对于值的补充方式。速度快，模糊边缘，平滑图像。
  >
  > - 高斯滤波器：void GaussianBlur( InputArray src, OutputArray dst, Size ksize,double sigmaX, double sigmaY = 0,int borderType = BORDER_DEFAULT );
  >
  >   > 卷积核是高斯分布的矩阵，对于不同的方向（x&&y）有不同的sigma值。针对高斯噪音（按照高斯分布的噪声）
  >
  > - 中值滤波器：void medianBlur( InputArray src, OutputArray dst, int ksize );
  >
  >   > 顾名思义，用矩阵的中间值来代替该点的像素值。针对椒盐噪声有奇效。

- 相机标定(2022.09.18)

  > 寻找相机的内外参数矩阵：
  >
  > > 内参包括切向畸变，和径向畸变
  > >
  > > 外参主要是旋转，平移 # 世界坐标到相机坐标到图像坐标的变化矩阵 #
  > >
  > > - 什么是世界坐标？
  > >
  > >     也就是真实世界的立体空间坐标，是一个三维坐标系
  > >     Ow-XwYwZw ：世界坐标系,描述相机位置,单位m
  > >
  > > - 什么是相机坐标？
  > >
  > >     根据透镜成像原理，将世界坐标在照相机内呈现，是一个三维坐标系
  > >     Oc-XcYcZc  ：相机坐标系,光心为原点,单位m
  > >
  > > - 什么是图像坐标？
  > >
  > >     将相机呈现的三维坐标投影到屏幕上，而建立的新坐标系，不含高程信息，是一个二维坐标系
  > >     o-xy ：图像坐标系,原点为成像平面中点,单位mm
  > >
  > > - 什么是像素坐标？
  > >
  > >     将投影的图像坐标离散抽样形成的做种图片，是一个二维的坐标系
  > >     uv ：像素坐标系,原点为图像左上角,单位pixel
  > > 标定就直接用opencv 的模板就好了其他应该不需要了

- 关于findcontours的报错：其中的vector<vecetor<point>> 只能是point类型，不能是point2f等（应该，只试了point2f不行） 

- solvePnP流程(2022.09.21)

  > 1.相机标定提取相机的内参矩阵和畸变矩阵
  >
  > 2.对测距的图像进行特征提取，即提取出想要测距的部分，用框框起来（目的：得到四个角点的坐标）
  >
  > 3.获取想要测距的物体的世界坐标（物体的尺寸大小和世界坐标原点求得）和2D坐标（上一步得到的坐标）
  >
  > 4.输入solvePnP(object_corners, 2D_corners, camera_matrix, distortion_coeff, rvec, tvec, flag)

- 打开大恒工业相机：

  > 用官方的单帧代码循环的到，再根据c开发手册转为彩色图片，具体原理不清楚（反正很多）![](/run/user/1000/doc/6a5888b6/2022-09-21 21-34-12 的屏幕截图.png)

- cmake学习（2022.09.23）

  >- add_executable(生成的可执行包名字  源文件1 源文件2 源文件3 ……)：编译源文件，生成可执行文件
  >
  > >```cpp
  > >add_executable(Demo ${DIR_SRCS}) ：取变量DIR_SRCS包含的源文件进行编译
  > >```
  >
  >- aux_source_directory(目录 变量名)：aux_source_directory(../ Dir_src)
  >
  > > ```cpp
  > > 该命令会查找指定目录下的所有源文件，然后将结果存进指定变量名
  > > ```
  >
  >- target_link_libraries(可执行文件名 链接库名)：为可执行文件链接库
  >
  > > ```cpp
  > > target_link_libraries(Demo MathFunctions)
  > > ```
  >
  >- add_library(链接库名 )：生成链接库
  >
  > > ```cpp
  > > aux_source_directory(. DIR_LIB_SRCS):将当前目录下的源文件包含进入变量
  > > add_library (MathFunctions ${DIR_LIB_SRCS})：将变量生成链接库
  > > ```
  >
  >- include_directories(包含头文件路径) ：可以在其他源文件直接引用，不加对应目录（2022.10.4）
  >
  >```cpp
  >include_directories ([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])
  >将指定目录添加到编译器的头文件搜索路径之下，指定的目录被解释成当前源码路径的相对路径。
  >```
  >
  >
  >
  >- 
  >
  >   
  >
  > 

- 

- cmake 项目（2022.09.24）

  > - 主函数所在目录需要有一个cmakeLists ，各个对应文件夹下也需要cmakelists，
  >
  >   ```cpp
  >   主函数的cmakelists:
  >   cmake_minimum_required(VERSION 3.0.0) #cmake所需版本号
  >   project(cam VERSION 0.1.0) # 项目名称以及版本号
  >                           
  >   set(OpenCV_INCLUDE_DIRS /usr/include/opencv4) #外界库导入名称，对应路径
  >   find_package(OpenCV REQUIRED) #找到上面对应的库
  >                           
  >   set(SRC          # 设置变量，并把对应要编译的源文件赋值给它。对于
  >       "main.cpp"
  >   )
  >   add_subdirectory(include)
  >   add_subdirectory(add)
  >   include_directories(${OpenCV_INCLUDE_DIRS})
  >   add_executable(cam ${SRC})
  >   target_include_directories(cam PRIVATE ./include)
  >   target_link_libraries(cam ${OpenCV_LIBS} ${MathFunctions})
  >   ```
  >

- (2022.10.07)

  > - vector.reverse(int n) 
  >
  >   > 为vector增加至少可以储存n个元素的空间，在后面push_back时，不再自动分配内存
  >
  > - enum 枚举类型
  >
  >   > 用法：enum typeName{ valueName1, valueName2, valueName3, ...... }; e.g : enum week{ Mon = 1, Tues, Wed, Thurs, Fri, Sat, Sun };
  >   >
  >   > 性质：值默认从0开始，step = 1,实际上就是int类型，在编译阶段替换，也就是常量了。
  >   >
  >   > 

- opencv CommandLine Parser（2022.10.08）

  > 现在只知道能做：用Key来充当要输入的文件目录集锦，用对应变量名来代替它，在文件开头能够比较清晰表现出来

- 

- 相机标定：（2022.9.18）

  > 运用opencv官方教程的相机标定cpp进行了对笔记本的摄像头实时标定，成功进行了标定。后面又在网上找了个对照片集的标定cpp，用手机拍摄了实验室的标定板子，相对手机的相机进行标定，但一开始就卡住了，一直卡在找角点的地方，最后跑出来就直接cv::findChessBoardConers失败，一搜发现是角点的个数设置不对，后面改了又跑，第一张图片秒出，但然后就又卡住了，程序也没报错，最后好一会跑出来第二张。怀疑是图片太大了，手机拍的每张都有3M，4M，然后就去把他们压缩成200~300KB的，但还是不行，还是最多跑出来两张。又用opencv官方教程的标定跑了几次，一样不行。又搜到可以用UMat 加速运算，跑了一遍还是不行。虽然照片集没成功，但实时的还是可以的，但效果不知道怎么样。
  >
  > 用手机上的图片标定的，数据看起来就比较离谱![](/run/user/1000/doc/3f1b2256/camera.png)

- 为了实践写的测距代码，用大恒相机进行标定，再实时测距查看效果（2022.09.21）

  >打开相机，把灰度转彩色，待续……把相机标定，然后测二维码吧，听说有现成的二维码识别代码可以直接用，嘿嘿![](/run/user/1000/doc/fd4f15c9/2022-09-21 21-33-56 的屏幕截图.png)

- **Real time pose estimation of a textured object**（结论：主要是solovePnPRANSAC():多点直线拟合的感觉，其余的都是为了做这个而提取的特征） 

  > - **(2022.10.09)** 编译出来效果很好，但具体代码还有待学习(我的评价是寄)
  
  > - pkg-config --modversion 包 (查看包的版本)
  >
  > - **(2022.10.12)** RANSAC算法：
  >
  >   > ![img](https://pic1.zhimg.com/v2-92f0ad1a9054d4bd19a759b7e3167bcc_r.jpg)
  >   >
  >   > 可以用来拟合直线，主要是解决外点和内点的问题。	
  >
  > - **(2022.10.14)** model 检测角点 和 实时检测的角点
  >
  >   > 把model角点和实时检测角点进行正反匹配，i.e. 分别作为queryset 和 trainset ,进行最适合的两个点匹配，对于不足的直接删除，再把正反匹配的再进行对应匹配，i.e. 是否是一个作为训练集时候，另一个作为查询集，准确判断结果，得到good_matches.
  >   
  > - **(2022.10.15)** 如何判断是内点还是外点：
  >
  >   > 用solvePnPRANSAC输入的3d和2d点进行RANSAC拟合，把拟合**用到的**判断为内点，未用到的判断为外点。
  >   >
  >   > 1. 对于detection的时候就是把采集的每一帧图像进行角点提取，描述符，和作为model 的模板yml储存的3d和2d点进行匹配，得到good_matchs,然后再用good_match的trainIdx和queryIdx进行提取，把标记的3d模型点用trainIdx提取作为3d点，把图像提取的2d角点作为2d点，建立对应关系，再把它们放入位姿估计RANSAC中进行拟合，得到内点，同时也得到了对应的旋转矩阵和平移矩阵，作为最终结果，不在内点中的角点就是外点。
  >   > 2. 对于画mesh，是先进行上面的操作，得到R,T（或者组合起来就是一个P,外参矩阵），再进行KF滤波，得到更**合理**的值，再对mesh的每一个3d点进行三维到二维投影，再在图像上画出来。
  >   > 3. KF滤波…… 有时间看看试试
  >   >
  
- kalman filter（2022.10.22）https://zhuanlan.zhihu.com/p/48876718

  >![](./kalman_filter.png)
  >
  >一维与多维：
  >
  >![](./X=Fx+bu.png)运动模型：X = Fx+Bu+W(过程噪声)
  >
  
- 粒子滤波（2022.10.19）

  > 由最基础的贝叶斯估计开始介绍，再引出蒙特卡罗采样，重要性采样，SIS粒子滤波，重采样，基本粒子滤波Generic Particle Filter，SIR粒子滤波，这些概念的引进，都是为了解决上一个概念中出现的问题而环环相扣的。最后给出几个在matlab和python中的应用，例程包括图像跟踪，滤波，机器人定位。：https://blog.csdn.net/heyijia0327/article/details/40899819

  - 贝叶斯滤波：

    > 从贝叶斯理论的观点来看，状态估计问题（目标跟踪、信号滤波）就是根据之前一系列的已有数据![This is the rendered form of the equation. You can not edit this directly. Right click will give you the option to save the image, and in most browsers you can drag the image onto your desktop or another program.](http://latex.codecogs.com/png.latex?%5Cinline%20y_%7B1%3Ak%7D)（后验知识）递推的计算出当前状态![This is the rendered form of the equation. You can not edit this directly. Right click will give you the option to save the image, and in most browsers you can drag the image onto your desktop or another program.](http://latex.codecogs.com/png.latex?x_%7Bk%7D)的可信度。这个可信度就是概率公式![This is the rendered form of the equation. You can not edit this directly. Right click will give you the option to save the image, and in most browsers you can drag the image onto your desktop or another program.](http://latex.codecogs.com/png.latex?%5Cinline%20p%5Cleft%20%28%20x_%7Bk%7D%7Cy_%7B1%3Ak%7D%20%5Cright%20%29)，它需要通过预测和更新两个步奏来递推的计算。
    >
    >**预测**过程是利用系统模型(状态方程1)预测状态的先验概率密度，也就是通过已有的先验知识对未来的状态进行猜测，即p( x(k)|x(k-1) )。**更新**过程则利用最新的测量值对先验概率密度进行修正，得到后验概率密度，也就是对之前的猜测进行修正

  - ​      

  
