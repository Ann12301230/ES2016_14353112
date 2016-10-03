###about DOL###
The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of applications onto the multiprocessor SHAPES architecture platform. 
The DOL consists of basically three parts:
- DOL Application Programming Interface.  
- DOL Functional Simulation.  
- DOL Mapping Optimization.

###Install###
*以下步骤来自实验文档，由于配置早已完成，只写下基本过程，不再截图*
1. 配置基础环境(vmware+ubuntu）
```
$	sudo apt-get update
$	sudo apt-get install ant
$    sudo apt-get install openjdk-7-jdk
$	sudo apt-get install unzip
```
2. 下载systemc和dol文件
```
sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
```
3. 解压文件
```
//新建dol的文件夹 
$	mkdir dol
//将dolethz.zip解压到 dol文件夹中
$	unzip dol_ethz.zip -d dol
//解压systemc
$	tar -zxvf systemc-2.3.1.tgz
```
4. 编译systemc
```
//解压后进入systemc-2.3.1的目录下
$	cd systemc-2.3.1
//新建一个临时文件夹objdir
$	mkdir objdir
//进入该文件夹objdir
$	cd objdir
//运行configure(能根据系统的环境设置一下参数，用于编译)
$	../configure CXX=g++ --disable-async-updates
//编译
$	sudo make install
//记录当前的工作路径(会输出当前所在路径，记下来，待会有用)
$	pwd
```
5. 编译dol
进入刚刚dol的文件夹
`
$	cd ../dol
`
修改build_zip.xml文件
```
/*找到下面这段话，就是说上面编译的systemc位置在哪里，
注意不要用浏览器打开，否则不可修改。*/
<property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
//把YYY改成上页pwd的结果（注意，对于  64位 系统的机器，lib-linux要改成lib-linux64）
```
编译
```
$	ant -f build_zip.xml all
若成功会显示build successful
```
接着可以试试运行第一个例子:
进入build/bin/mian路径下
` $	cd build/bin/main `
然后运行第一个例子
`
$	ant -f runexample.xml -Dnumber=1
`
得到下图：
![第一个例子](http://upload-images.jianshu.io/upload_images/3195764-925a17d74a7ba8b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
至此DOL配置成功！
###感想与心得###
本次实验比较简单，按照步骤基本上都能成功。
但是也遇上了一些问题：
Q:下载文件时网速慢，甚至停止下载，但本地网络没有任何问题。
A: 直接在命令行下载会访问ubuntu的源，一般都是需要连入国外的网，所以会比较慢，发现网速慢时中止下载再重新开始，多重复几次即可发现网速变快。
Q：打开命令行输入语句发现不执行，总是报错找不到某个目录。
A：上次关闭虚拟机时有任务被迫中止执行，使用清除语句清除掉或者重启虚拟机即可。
Q：虚拟机中下载markdown编辑器但无法打中文
A：可在虚拟机外部写好markdown文件再用putty或者filezilla传入（直接拖入可能会有损失）
