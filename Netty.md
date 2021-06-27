# 1. 课程介绍

![课程要求](D:\Desktop\markdown\尚硅谷netty\课程要求.png)

## 1.1 Netty介绍



![netty介绍](D:\Desktop\markdown\尚硅谷netty\netty介绍.png)



关于同步和异步

![关于同步和异步](D:\Desktop\markdown\尚硅谷netty\关于同步和异步.png)

同步：浏览器发出请求后，要等待服务器相应回来才能继续做其他的事情

异步：发出请求之后，无需等响应，可以去做其他的事情（运行程序的后面流程），也可以发送其他请求，相应由对应的回调函数处理

![底层的大概模型](D:\Desktop\markdown\尚硅谷netty\底层的大概模型.png)



---

# 2. I/O模型

## 2.1 基本说明

![IO基本说明](D:\Desktop\markdown\尚硅谷netty\IO基本说明.png)

1. BIO：读写的时候可能会阻塞

   ![BIO](D:\Desktop\markdown\尚硅谷netty\BIO.png)

2. NIO

   ![NIO](D:\Desktop\markdown\尚硅谷netty\NIO.png)

   netty目前是基于NIO的，而非AIO，AIO目前没有得到广泛应用（那netty到底是同步还是异步）

![适用场景](D:\Desktop\markdown\适用场景.png)



## 2.2 BIO 

1. 基本介绍

   ![BIO基本介绍](D:\Desktop\markdown\尚硅谷netty\BIO基本介绍.png)

![BIO简单流程](D:\Desktop\markdown\尚硅谷netty\BIO简单流程.png)

2. 问题说明

   ![BIO存在问题](D:\Desktop\markdown\尚硅谷netty\BIO存在问题.png)

## 2.3 NIO

### 1. 基本介绍

![NIO基本介绍](D:\Desktop\markdown\尚硅谷netty\NIO基本介绍)

​	注：Channel相当于BIO中的socket

​		更细致一点的NIO模型

![NIO细致模型](D:\Desktop\markdown\尚硅谷netty\NIO细致模型.png)

​	注：buffer帮助完成非阻塞

![NIO基本介绍2](D:\Desktop\markdown\尚硅谷netty\NIO基本介绍2.png)

​	注：上图中5的红字线程，指的是上图中**server创建selector的线程**，该线程不会阻塞在一个通道的读写上，可以管理多个通道，也可以做别的事情

![BIO/NIO比较](D:\Desktop\markdown\尚硅谷netty\BIO和NIO比较.png)

### 2. Buffer的基本使用

### 3. NIO三大核心组件关系

![三组件关系图](D:\Desktop\markdown\尚硅谷netty\三组件关系图.png)

关系图说明：

1. 每个channel都会对应一个buffer
2. selector对应一个线程，一个线程对应多个channel（连接）
3. 该图反应了有三个channel注册到了该selector程序
4. 程序切换到哪个channel由**事件(event)**决定(事件驱动)，event是一个重要概念
5. selector会根据不同的事件，在各个通道上切换
6. buffer就是一个内存块，底层是一个数组
7. 数据的读取或写入都是通过buffer，这个和BIO有本质不同，BIO中要么是输入流，或输出流，不能双向；但是NIO的buffer是可以读也可以写，但需要调用一次filp方法切换
8. channel是双向的，可以反映底层操作系统的情况，比如linux，底层操作系统通道就是双向的

## 2.4 NIO三大组件

### 1. Buffer

#### 1.基本介绍

![buffer基本介绍.png](D:\Desktop\markdown\尚硅谷netty\buffer基本介绍.png)

![buffer核心参数.png](D:\Desktop\markdown\尚硅谷netty\buffer核心参数.png)

---

![自类.png](D:\Desktop\markdown\尚硅谷netty\自类.png)

#### 2. 核心参数

![image-20200812110621930](D:\Desktop\markdown\尚硅谷netty\参数介绍.png)

​	

limit和position的区别？ 看一下flip()函数的代码就知道了

```java
public final Buffer flip(){
    limit = position;
    position = 0;
    mark = -1;
    return this;
}
```

例如：写操作后，position走到n的位置，这时候数组写入了n-1，执行flip方法，标志要开始读数据，这是把limit设为刚才的position，告诉读的时候不能超过limit的限定的范围

![buffer相关方法.png](D:\Desktop\markdown\尚硅谷netty\buffer相关方法.png)

**ByteBuffer**

![](C:\Users\xinbo.ju\AppData\Roaming\Typora\typora-user-images\image-20200812110818393.png)

put还可以传入byte[]，比较常用



### 2. Channel

#### 1. 基本介绍



![image-20200812111208371](D:\Desktop\markdown\尚硅谷netty\通道介绍.png)

![image-20200812111645389](D:\Desktop\markdown\尚硅谷netty\通道介绍2.png)

![socketChannel在模型上对应的位置.png](D:\Desktop\markdown\尚硅谷netty\socketChannel在模型上对应的位置.png)





![filechannel.png](D:\Desktop\markdown\尚硅谷netty\filechannel.png)

#### 2. 例子

![写入举例.png](D:\Desktop\markdown\计算机网\写入举例.png)

1. 写入文件

   ```java
   public static void main(String[] args) throws IOException {
       String str = "abc";
       // 创建一个输出流——》channel
       FileOutputStream fileOutputStream = new FileOutputStream("a.txt");
   
       // 通过输出流获取对应的filechannel
       // 这个filechannel 真实类型是 filechanelimpl
       FileChannel fileChannel = fileOutputStream.getChannel();
   
       // 创建缓冲区bytebuffer
       ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
   
       // 将str放入bytebuffer
       byteBuffer.put(str.getBytes());
   
       // 对baffer反转，反转后才能写
       byteBuffer.flip();
   
       fileChannel.write(byteBuffer);
       fileOutputStream.close();
   }
   ```

2. 读取文件

```java
public static void main(String[] args) throws IOException {
    File file = new File("a.txt");
    FileInputStream inputStream = new FileInputStream(file);

    FileChannel fileChannel = inputStream.getChannel();
	
    // 注意此处用file的长度来决定allocate多少内存给bytebuffer
    ByteBuffer byteBuffer = ByteBuffer.allocate((int) file.length());

    fileChannel.read(byteBuffer);
    byteBuffer.flip();
	
    // array方法直接返回一个byte数组
    System.out.println(new String(byteBuffer.array()));

    inputStream.close();
}
```

3. 读文件并且复制到其他文件

```java
FileOutputStream outputStream = new FileOutputStream(new File("b.txt"));
FileChannel fileChannel2 = outputStream.getChannel();
FileInputStream inputStream = new FileInputStream(new File("a.txt"));
FileChannel fileChannel1 = inputStream.getChannel();

ByteBuffer byteBuffer = ByteBuffer.allocate(512);

while (true){

    byteBuffer.clear(); // 清空buffer
    int read = fileChannel1.read(byteBuffer);
    if (read == -1) break;

    byteBuffer.flip();

    fileChannel2.write(byteBuffer);
}
```





















## 2.0 书籍补充

### 1. UNIX中5种I/O模型

1. 阻塞I/O：进程空间调用recvfrom，这个系统调用直到数据包到达且被复制到应用进程的缓冲区，或发生错误才返回，在此期间一直等待。进程从调用recvfrom开始，到它返回的时间内都是阻塞的

2. 非阻塞I/O：recvfrom从应用层到内核，如果缓冲区没有数据，就返回一个EWOULDBLOCK错误，对非阻塞I/O轮询检查这个状态，看是否有数据到来；也就是反复调用recvfrom

   注：阻塞和非阻塞是指**server**端，进程访问的数据如果没有就绪，进程(**这个地方应该是server端对应的线程**)是否需要等待；相当于函数内部实现，未就绪的时候是直接返回还是等待就绪

   疑问：但是上面两种I/O，进程还是阻塞与recvfrom调用，因此**关键在于server端是否立刻返回？**

   

3. I/O复用模型：linux提供select/poll，进程将一个或多个fd（文件描述符）传递给select或poll系统调用，**阻塞在select调用上**，这样select/poll可以帮我们侦测多个fd是否处于就绪状态；select/poll顺序扫描fd是否就绪，并且支持的fd数量有限，因此使用上有制约

   linux还提供epoll，基于事件驱动代替顺序扫描，性能更高，当fd就绪，立刻回调函数rollback

4. 信号驱动I/O模型：开启套接口信号驱动I/O功能，通过系统调用sigaction执行信号处理函数（此系统调用立即返回，**进程继续工作**，非阻塞的）；当前数据就绪时，为该进程生成一个SIGIO信号，通过信号回调通知应用程序（进程）调用recvfrom来读取数据，（也就说，收到SIGIO信号调用recvfrom时，进程才阻塞，等到数据复制到进程缓冲区）

5. 异步I/O：告诉内核某个操作，内核完成后（包括数据从内核复制到自己的缓冲区）通知我们

   与信号驱动的主要区别：

   - 信号驱动I/O由系统通知我们合适可以开始一个IO
   - 异步IO由内核通知我们IO何时已经完成

   注：同步异步是指，client发起调用后，是否一直等待调用结果；异步的话，发起调用就可以去做其他事情了，系统会告诉我们调用何时已经完成

### 2. 同步异步，阻塞非阻塞

1. 同步异步：数据访问的时候，进程是否阻塞，也就是说，发起调用的进程是否需要等待结果返回，才能做其他的事情
2. 阻塞非阻塞：应用程序的调用是否立即返回，也就是server端是否立即返回相应（或者是直接返回结果，或者返回没准备好的信号）

### 3. I/O多路复用

​	I/O编程时，如果需要同时处理多个客户端接入请求，可以利用多线程或I/O多路复用；I/O多路复用把多个I/O的阻塞服用到同一个select的阻塞上，从而系统可以在**单线程的情况下同时处理多个客户端请求**

​	相比于多线程/多进程，I/O多路复用的优势是系统开销小，降低系统维护工作量，节省系统资源



   	1. 应用场景：
       - 需要同时处理多个处于监听状态或多个连接状态的套接字
       - 需要同时处理多种网络协议的套接字
  	2. 之前很久都是用select，但是有缺陷，为了克服select的缺陷，epoll做出了很多改进

### 4. epoll的改进

1. 一个进程打开的socket描述符（FD）不受限制（仅受限于操作系统的最大文件句柄数）

   ​	select最大缺陷就是单个进程的FD数目有限制，FD_SETSIZE默认为1024，这个值可以修改但是网络效率会下降

   ​	epoll的FD上限是操作系统的最大文件句柄数，具体可以通过cat /proc/sys/fs/file-max查看，通常这个值跟系统内存关系比较大，1GB内存的话大概是10w左右

2. I/O效率不会随着FD增加而线性下降

   ​	select/poll另一个致命弱点，当socket集合很大时，由于网络延时或链路空闲，任一时刻只有少部分socket活跃，但select/poll每次调用都会**线性扫描**集合，导致效率线性下降

   ​	epoll没有此问题，它只对活跃的socket操作，因为内核实现中，epoll是根据每个fd上的callback函数实现的，只有活跃的socket才会主动调用callback；这点上epoll实现了伪AIO

3. 使用mmap加速内核与用户空间的消息传递

   ​	select/poll/epoll都需要内核把FD消息通知给用户空间，如果避免不必要的内存复制就很重要；

   ​	epoll是通过内核和用户空间mmap同一块内存来实现的

4. epoll的api更简单

   

   

