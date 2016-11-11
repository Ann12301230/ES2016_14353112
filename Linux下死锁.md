####产生死锁的代码
    //Deadlock.java
    class A{
	synchronized void methodA(B b){
		b.last();
	}
	synchronized void last(){
		System.out.println("Inside A.last()");
	}
    }
    class B{
	synchronized void methodB(A a){
		a.last();
	}
	synchronized void last(){
		System.out.println("Inside B.last()");
	}
    }

    class Deadlock implements Runnable{
	A a = new A();
	B b = new B();
	Deadlock(){
		Thread t = new Thread(this);
		int count=200000;
		t.start();
		while(count-- >0);
		a.methodA(b);
	}
	public void run(){
		b.methodB(a);
	}
	public static void main(String args[]){
		new Deadlock();
	}
    }
之后用预处理文件Deadlock.bat运行
    #!/bin/bash
    for((c=1;c<=300;c++))
    do 
	  echo "$c times"
	  java Deadlock
    done
注：linux下运行bat文件：
1. chmod +x 文件名
先用上面的命令把批处理文件修改为可执行文件。
2. ./文件名
执行该文件。
####运行结果

![blocked in the 154th iterations.png](http://upload-images.jianshu.io/upload_images/3195764-2a28d34724dfa99e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果发现没有死锁，可以把每个锁占有时间count延长，或者加大迭代次数c。
####产生死锁的必要条件
- **互斥：**指进程对所分配到的资源进行排它性使用，即在一段时间内某资源只由一个进程占用。如果此时还有其它进程请求资源，则请求者只能等待，直至占有资源的进程用毕释放。
- **占有等待：**指进程已经保持占有一个资源，但又提出了新的资源请求，而该资源已被其它进程占有，此时请求进程阻塞，但又对自己已获得的其它资源保持不放，等待申请的资源。
- **不可抢占：**指进程已获得的资源，在未使用完之前，不能被抢占，只能在使用完时由自己释放。
- **循环等待：**指在发生死锁时，必然存在锁和资源的环链，即进程A占有锁A申请锁B，进程B占有锁B申请锁C....进程N占用锁N申请锁A，这样所有进程都不能立即得到资源，形成死锁。

####代码分析
首先是synchronized 关键字，使用这个关键字来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。

这样使得两个类的last函数相当于一个资源锁，不能同时被并行访问。
在主函数中新建一个deadlock对象，这个对象的构造函数中会建一个线程，当线程调用start（）函数时，会运行run（）函数，run函数中使得b占用a的锁，然后释放。接着回到t.start()的下一步，在设置的count毫秒内使得a占有b的锁。

使用预处理文件循环跑这个代码，使得有可能出现下面的情况：
b.methodB(a)的同时a.methodA(b)，在这两个函数内部调用的是对方的last函数，但是由于synchronized 关键字使得只能有一个线程访问对象的一个synchronized函数，其他的被阻塞，所以造成了：a调用自己的methodA函数，使得自己的last函数被阻塞，b也一样，这样a和b同时在等待对方的last函数，导致死锁。
