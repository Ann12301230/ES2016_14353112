##ROS简介
  ROS系统是起源于2007年斯坦福大学[人工智能](http://lib.csdn.net/base/machinelearning)实验室的项目与机器人技术公司Willow Garage的个人机器人项目（Personal Robots Program）之间的合作，2008年之后就由Willow Garage来进行推动。
  ROS是开源的，是用于机器人的一种后操作系统，或者说次级操作系统。它提供类似操作系统所提供的功能，包含硬件抽象描述、底层驱动程序管理、共用功能的执行、程序间的消息传递、程序发行包管理，它也提供一些工具程序和库用于获取、建立、编写和运行多机整合的程序。
##ROS安装
  安装建议按照官网的步骤来，当然有许多博客上做了些优化和删减，基本上按照步骤来做不会有什么问题。
  ROS的版本丰富，用的比较多的是indigo和jade版本，jade版本比较新，但是总的来说indigo的支持更多，并且indigo版本的navigation比较完善，所以建议装indigo版本。
  参考博客：http://blog.csdn.net/sunbibei/article/details/45279385
  如果希望对ROS有个更深入的理解，建议看看这篇文章：
  https://cse.sc.edu/~jokane/agitr/%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%EF%BC%88ROS%EF%BC%89%E6%B5%85%E6%9E%90.pdf
##安装完成后的测试
  想要在ROS上运行，首先必须新建一个catkin的workplace：
       ```mkdir /.../catkin_ws/src
     cd catkin_ws/src
     catkin_init_workspace```
  尽管这个workplace是空的（文件夹src中并没有文件，只有一个CMakeList.txt），我们仍然可以建立这个工作空间并初始化。
            ```$ cd ~/catkin_ws/
      $ catkin_make  ```
  catkin_make命令对于在catkin工作空间工作来说是一个非常方便的工具。如果你查看当前目录下你会发现当前目录应该有一个“build”和一个“devel”文件夹。
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3195764-34b2b65990203c00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  更新setup.*sh文件：
  ```source devel/setup.bash ```
  为了确认你的工作空间已经被至于最顶层，确认环境变量ROS_PACKAGE_PATH包含了你所在的目录：
  ```echo $ROS_PACKAGE_PATH```
 
>如果出现类似于
/home/youruser/catkin_ws/src:/opt/ros/kinetic/share:/opt/ros/kinetic/stacks
表明你已经建立一个简单的工作空间了。

  接下来开始运行例子：
1. roscore
  roscore是你在运行所有ROS程序前首先要运行的命令。
```$ roscore```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3195764-b0a1e6c16ceb4a94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 再打开一个Terminal,输入以下命令.开启一个小乌龟界面.
```  $ rosrun turtlesim turtlesim_node```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3195764-b89fb76c4d21db4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 再打开一个Terminal,输入以下命令.接受键盘输入,控制小乌龟移动.
```$ rosrun turtlesim turtle_teleop_key```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3195764-bc00bc1c9f7a5d4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####至此，ROS的配置及测试完成####
######可能遇到的错误
运行roscore时，可能会出现无法访问local host的情况。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3195764-cb398801348a2f6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这时，可以在bashrc文件中加入ROS_IP:
```$ gedit ~/.bashrc```
 在最后加上：
```export ROS_IP=xxx.xxx.xxx.xxx//你的虚拟机ip```
（1） 查看虚拟机ip可以用ifconfig
（2） 等号两旁不要加空格

完成这两步后，关闭terminal，重新打开并运行roscore即可。
更多问题参考：http://www.aichengxu.com/view/998814
