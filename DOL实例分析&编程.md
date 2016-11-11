DOL提供了13个example让用户理解/运行DOL实例，本次实验需要理解并修改example1和example2，达到以下要求：
1. 修改example2，让3个square模块变成2个
2. 修改example1，使其输出3次方数
##修改example1##
example1中包含生产者、平方模块、消费者、通道C1与C2，其中生产者产生一个0-20的数，经过square模块平方，送给消费者，消费者将数据打印出。
![example1](http://upload-images.jianshu.io/upload_images/3195764-7d7686cf45ff70b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

example本身就是二次方函数
> square.c
...
i = i*i; 
...

所以直接将这一句修改为```i=i*i*i```即可，这个主要是熟悉一下编译运行的过程，每次修改之后，要先build: ```ant -f build_zip.xml all```,之后才能运行。需要在build/bin/mian路径下
```$	cd build/bin/main```
然后运行第一个例子
```$	ant -f runexample.xml -Dnumber=1```
运行结果：
  
![三次方](http://upload-images.jianshu.io/upload_images/3195764-990071862bd78d02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##修改example2##
example2在example1的基础上，添加了两个中间模块及其通道，相当于得到生产者的数的8次方：
![example2.png](http://upload-images.jianshu.io/upload_images/3195764-c2cb1300752cda20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
看看example2代码相对于example1有什么修改：
> 
![iteration time](http://upload-images.jianshu.io/upload_images/3195764-fbb93ad9e9fdf994.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![square.png](http://upload-images.jianshu.io/upload_images/3195764-85cd37f44a72cee7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
首先是通过迭代生成三个square模块，以及六个接口。
![connection.png](http://upload-images.jianshu.io/upload_images/3195764-4b3eb3ea991f9ebd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其次是将对应的接口连起来，形成通道。

这样看起来，其实就是通过循环生成模块和对应接口，那么想修改模块的个数就非常简单，将迭代数N改为2即可。
修改后结果：

![result.png](http://upload-images.jianshu.io/upload_images/3195764-434ef179e3288448.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![dot.png](http://upload-images.jianshu.io/upload_images/3195764-aeffaf0e23e3e430.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到，square模块从三个变成了两个，结果也就从8次方变为了4次方。
###实验感想
1. 修改后编译运行example1时，结果总是未修改的结果，这是因为在上一次实验中已经编译运行过example1了，build/bin/main/中包含了未修改的结果，所以要将build/bin/mian中的结果删掉重新编译。
2. 修改中间模块时，可以用迭代，也可以自己手写，但是一定要主要各个接口一定要对应上，并且.c/.h代码中的接口也要和xml中的对应。

