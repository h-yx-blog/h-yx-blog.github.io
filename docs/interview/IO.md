
# IO知识点及面试题
## java 中 IO 流分为几种?
- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。


Java Io流共涉及40多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java I0流的40多个类都是从如下4个抽象类基类中派生出来的。

- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。



1. 按操作方式分类结构图：
![img](http://hyxjackson.gitee.io/picturesave/aHR0cHM6Ly9teS1ibG9nLXRvLXVzZS5vc3MtY24tYmVpamluZy5hbGl5dW5jcy5jb20vMjAxOS02L0lPLSVFNiU5MyU4RCVFNCVCRCU5QyVFNiU5NiVCOSVFNSVCQyU4RiVFNSU4OCU4NiVFNyVCMSVCQi5wbmc)

2. 按操作对象分类结构图
![img](http://hyxjackson.gitee.io/picturesave/aHR0cHM6Ly9teS1ibG9nLXRvLXVzZS5vc3MtY24tYmVpamluZy5hbGl5dW5jcy5jb20vMjAxOS02L0lPLSVFNiU5MyU4RCVFNCVCRCU5QyVFNSVBRiVCOSVFOCVCMSVBMSVFNSU4OCU4NiVFNyVCMSVCQi5wbmc)




## 1. 同步阻塞IO（BIO）
- A拿着一支鱼竿在河边钓鱼，并且一直在鱼竿前等，在等的时候不做其他的事情，十分专心。只有鱼上钩的时，才结束掉等的动作，把鱼钓上来。

- 在内核将数据准备好之前，系统调用会一直等待所有的套接字，默认的是阻塞方式。
![BIO](https://img-blog.csdnimg.cn/20200411182053967.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)






## 2. 同步非阻塞NIO（noblocking I/O）
B也在河边钓鱼，但是B不想将自己的所有时间都花费在钓鱼上，在等鱼上钩这个时间段中，B也在做其他的事情（一会看看书，一会读读报纸，一会又去看其他人的钓鱼等），但B在做这些事情的时候，每隔一个固定的时间检查鱼是否上钩。一旦检查到有鱼上钩，就停下手中的事情，把鱼钓上来。 B在检查鱼竿是否有鱼，是一个轮询的过程
![NIO](https://img-blog.csdnimg.cn/20200411182104265.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)

> NIO是JDK1.4提供的操作，他的流还是流，没有改变，服务器实现的还是一个连接一个线程，当是：客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4之后开始支持。



## NIO中的一些概念
1. 什么是通道（Channel）
- Channel是一个对象，可以通过它读取和写入数据。通常我们都是将数据写入包含一个或者多个字节的缓冲区，然后再将缓存区的数据写入到通道中，将数据从通道读入缓冲区，再从缓冲区获取数据。

- Channel 类似于原I/O中的流（Stream），但有所区别：流是单向的，通道是双向的，可读可写。流读写是阻塞的，通道可以异步读写。



2. 什么是选择器（Selector）
Selector可以称他为通道的集合，每次客户端来了之后我们会把Channel注册到Selector中并且我们给他一个状态，在用死循环来环判断(判断是否做完某个操作，完成某个操作后改变不一样的状态)状态是否发生变化，知道IO操作完成后在退出死循环


3. 什么是Buffer（缓冲区）
- Buffer 是一个缓冲数据的对象， 它包含一些要写入或者刚读出的数据。

- 在普通的面向流的 I/O 中，一般将数据直接写入或直接读到 Stream 对象中。当是有了Buffer（缓冲区）后，数据第一步到达的是Buffer（缓冲区）中

- 缓冲区实质上是一个数组(底层完全是数组实现的，感兴趣可以去看一下)。通常它是一个字节数组，内部维护几个状态变量，可以实现在同一块缓冲区上反复读写（不用清空数据再写）。



## 3. 异步AIO（asynchronous I/O）
- C也想钓鱼，但C有事情，于是他雇来了D、E、F，让他们帮他等待鱼上钩，一旦有鱼上钩，就打电话给C，C就会将鱼钓上去。

- 当应用程序请求数据时，内核一方面去取数据报内容返回，另一方面将程序控制权还给应用进程，应用进程继续处理其他事情，是一种非阻塞的状态。
![AIO](https://img-blog.csdnimg.cn/20200411182111687.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)

## 4. 信号驱动IO（signal blocking I/O）
- G也在河边钓鱼，但与A、B、C不同的是，G比较聪明，他给鱼竿上挂一个铃铛，当有鱼上钩的时候，这个铃铛就会被碰响，G就会将鱼钓上来。
- 信号驱动IO模型，应用进程告诉内核：当数据报准备好的时候，给我发送一个信号，对SIGIO信号进行捕捉，并且调用我的信号处理函数来获取数据报。
![信号驱动IO](https://img-blog.csdnimg.cn/20200411182126550.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)

## 5. IO多路复用（I/O multiplexing）
也叫IO多路转接
- H同样也在河边钓鱼，但是H生活水平比较好，H拿了很多的鱼竿，一次性有很多鱼竿在等，H不断的查看每个鱼竿是否有鱼上钩。增加了效率，减少了等待的时间。
- IO多路转接是多了一个select函数，select函数有一个参数是文件描述符集合，对这些文件描述符进行循环监听，当某个文件描述符就绪时，就对这个文件描述符进行处理。
- IO多路转接是属于阻塞IO，但可以对多个文件描述符进行阻塞监听，所以效率较阻塞IO的高。
![IO多路复用](https://img-blog.csdnimg.cn/20200411182136966.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)



> 在多路复用IO模型中，会有一个线程不断去轮询多个socket的状态，只有当socket真正有读写事件时，才真正调用实际的IO读写操作。
因为在多路复用IO模型中，只需要使用一个线程就可以管理多个socket，系统不需要建立新的进程或者线程，也不必维护这些线 程和 进程，并且只有在真正有socket读写事件进行时，才会使用IO资源，所以它大大减少了资源占用。

- select：轮询所有的socket

- poll：轮询所有的socket，但是和select相比，没有最大连接数的限制，原因是它是基于链表存储的

- epoll：只会轮询发生了IO请求的socket。虽然连接数有上限，但是很大，1G内存的机器上可以打开10W左右的连接；


### 多路复用IO小结
- I/O 多路复用模型是利用select、poll、epoll可以同时监察多个流的 I/O 事件的能力，在空闲的时候，会把当前线程阻塞掉，当有一个或多个流有I/O事件时，就从阻塞态中唤醒，于是程序就会轮询一遍所有的流（epoll是只轮询那些真正发出了事件的流），依次顺序的处理就绪的流，这种做法就避免了大量的无用操作。这里“多路”指的是多个网络连接，“复用”指的是复用同一个线程。

- 采用多路 I/O 复用技术可以让单个线程高效的处理多个连接请求（尽量减少网络IO的时间消耗），且Redis在内存中操作数据的速度非常快（内存内的操作不会成为这里的性能瓶颈），主要以上两点造就了Redis具有很高的吞吐量。

- IO多路复用。“多路”指的是多个网络连接客户端，“复用”指的是复用同一个线程（单线程）。I/O多路复用是使用一个线程来检查多个socket的状态。在单个线程中通过记录跟踪每一个socket的状态来管理处理多个I/O流。



## 面试题
1. BIO 和 NIO 作为 Server 端，当建立了 10 个连接时，分别产生多少个线程？

    答案： 因为传统的 IO 也就是 BIO 是同步线程堵塞的，所以每个连接都要分配一个专用线程来处理请求，这样 10 个连接就会创建 10 个线程去处理。而 NIO 是一种同步非阻塞的 I/O 模型，它的核心技术是多路复用，可以使用一个链接上的不同通道来处理不同的请求，所以即使有 10 个连接，对于 NIO 来说，开启 1 个线程就够了。
2. 三个模型的一些问题
    
    2.1 同步阻塞--为什么说BIO是同步阻塞的呢？
    
    答：针对磁盘文件读写IO操作来说，因为用BIO的流读写文件，例如FileInputStrem，必须等着完成了这次IO才能返回。

    2.2 同步非阻塞--为什么说NIO是同步非阻塞？

    答：因为无论多少客户端都可以接入服务端，客户端接入并不会耗费一个线程，只会创建一个连接，然后注册到selector上去，一个selector线程不断的轮询所有的socket连接，发现有事件了就通知你，然后你就启动一个线程处理一个请求即可，这个过程的话就是非阻塞的。但是这个处理的过程中，你还是要先读取数据，处理，再返回的，这是个同步的过程。

    2.3 异步非阻塞--为什么说AIO是异步非阻塞？

    答：当基于AIO的api去读写文件时，发起一个请求之后，等读写完成后， 操作系统会来回调你的接口， 告诉你操作完成。在这期间不需要等待， 也不需要去轮询判断操作系统完成的状态，你可以去干其他的事情。同步还得主动去轮询操作系统，异步就是操作系统反过来通知你，所以说 AIO就是异步非阻塞的。

3.  BIO,NIO,AIO 有什么区别?
    
    3.1 简答：

- BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
- NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
- AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

    3.2 详细回答

- BIO (Blocking I/O): 同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的 I/O 并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。
- NIO (New I/O，也可叫Non blocking I/O): NIO是一种同步非阻塞的I/O模型，在Java 1.4 中引入了NIO框架，对应 java.nio 包，提供了 Channel , Selector，Buffer等抽象。NIO中的N可以理解为Non-blocking，不单纯是New。它支持面向缓冲的，基于通道的I/O操作方法。 NIO提供了与传统BIO模型中的 Socket 和 ServerSocket 相对应的 SocketChannel 和 ServerSocketChannel 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发
- AIO (Asynchronous I/O): AIO 也就是 NIO 2。在 Java 7 中引入了 NIO 的改进版 NIO 2,它是异步非阻塞的IO模型。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。AIO 是异步IO的缩写，虽然 NIO 在网络操作中，提供了非阻塞的方法，但是 NIO 的 IO 行为还是同步的。对于 NIO 来说，我们的业务线程是在 IO 操作准备好时，得到通知，接着就由这个线程自行进行 IO 操作，IO操作本身是同步的。查阅网上相关资料，我发现就目前来说 AIO 的应用还不是很广泛，Netty 之前也尝试使用过 AIO，不过又放弃了。


4. Files的常用方法都有哪些？

- Files. exists()：检测文件路径是否存在。

- Files. createFile()：创建文件。
- Files. createDirectory()：创建文件夹。
- Files. delete()：删除一个文件或目录。
- Files. copy()：复制文件。
- Files. move()：移动文件。
- Files. size()：查看文件个数。
- Files. read()：读取文件。
- Files. write()：写入文件。


5. 同步和异步的概念描述的是用户线程与内核的交互方式：同步是指用户线程发起IO请求后需要等待或者轮询内核IO操作完成后才能继续执行；而异步是指用户线程发起IO请求后仍继续执行，当内核IO操作完成后会通知用户线程，或者调用用户线程注册的回调函数。

阻塞和非阻塞的概念描述的是用户线程调用内核IO操作的方式：阻塞是指IO操作需要彻底完成后才返回到用户空间；而非阻塞是指IO操作被调用后立即返回给用户一个状态值，无需等到IO操作彻底完成



6. 多路复用是阻塞的还是非阻塞的
- 如果select是靠轮询判断是否有资源可用的，那就是 非阻塞的

- 如果select发现没有可用资源，直接线程挂起了(程序属于是wait相关函数)，等待系统唤醒，那就是 阻塞。

事实上，除了异步IO，其他IO在从内核空间拷贝数据到用户空间这个阶段，用户线程都是被阻塞的。
也就说在异步IO模型中，IO操作的两个阶段都不会阻塞用户线程，这两个阶段都是由内核自动完成，然后发送一个信号告知用户线程操作已完成。用户线程中不需要再次调用IO函数进行具体的读写。这点是和信号驱动模型有所不同的，在信号驱动模型中，当用户线程接收到信号表示数据已经就绪，然后需要用户线程调用IO函数进行实际的读写操作；而在异步IO模型中，收到信号表示IO操作已经完成，不需要再在用户线程中调用IO函数进行实际的读写操作。



![img](http://hyxjackson.gitee.io/picturesave/142332187256396.png)

7. 请你说说IO多路复用（select 、poll、epoll）
    
    IO多路复用指的是单个进程或者线程能同时处理多个IO请求，select，epoll，poll是LinuxAPI提供的复用方式。本质上由操作系统内核缓冲IO数据，使得单个进程线程能监视多个文件描述符。select是将装有文件描述符的集合从用户空间拷贝到内核空间，底层是数组，poll和select差距不大，但是底层是链表，这就代表没有上限，而select有数量限制。epoll则是回调的形式，底层是红黑树，避免轮询，时间复杂度从O（n）变为O（1）
## 代码示例
BIO、NIO等的代码示例请参考
https://blog.csdn.net/weixin_43122090/article/details/105297786
里面的 “网络操作IO编程演变历史”这一小节，通俗易懂，非常推荐


> 本篇参考： https://blog.csdn.net/weixin_43122090/article/details/105297786 
https://blog.csdn.net/Allen_Walker_QAQ/article/details/80642274 
https://blog.csdn.net/weixin_70730532/article/details/125657676