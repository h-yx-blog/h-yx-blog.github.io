
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




## 1.同步阻塞IO（BIO）
- A拿着一支鱼竿在河边钓鱼，并且一直在鱼竿前等，在等的时候不做其他的事情，十分专心。只有鱼上钩的时，才结束掉等的动作，把鱼钓上来。

- 在内核将数据准备好之前，系统调用会一直等待所有的套接字，默认的是阻塞方式。
![BIO](https://img-blog.csdnimg.cn/20200411182053967.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)






## 同步非阻塞NIO（noblocking I/O）
B也在河边钓鱼，但是B不想将自己的所有时间都花费在钓鱼上，在等鱼上钩这个时间段中，B也在做其他的事情（一会看看书，一会读读报纸，一会又去看其他人的钓鱼等），但B在做这些事情的时候，每隔一个固定的时间检查鱼是否上钩。一旦检查到有鱼上钩，就停下手中的事情，把鱼钓上来。 B在检查鱼竿是否有鱼，是一个轮询的过程
![NIO](https://img-blog.csdnimg.cn/20200411182104265.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)


## 异步AIO（asynchronous I/O）
- C也想钓鱼，但C有事情，于是他雇来了D、E、F，让他们帮他等待鱼上钩，一旦有鱼上钩，就打电话给C，C就会将鱼钓上去。

- 当应用程序请求数据时，内核一方面去取数据报内容返回，另一方面将程序控制权还给应用进程，应用进程继续处理其他事情，是一种非阻塞的状态。
![AIO](https://img-blog.csdnimg.cn/20200411182111687.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)

## 信号驱动IO（signal blocking I/O）
- G也在河边钓鱼，但与A、B、C不同的是，G比较聪明，他给鱼竿上挂一个铃铛，当有鱼上钩的时候，这个铃铛就会被碰响，G就会将鱼钓上来。
- 信号驱动IO模型，应用进程告诉内核：当数据报准备好的时候，给我发送一个信号，对SIGIO信号进行捕捉，并且调用我的信号处理函数来获取数据报。
![信号驱动IO](https://img-blog.csdnimg.cn/20200411182126550.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)

## IO多路复用（I/O multiplexing）
也叫IO多路转接
- H同样也在河边钓鱼，但是H生活水平比较好，H拿了很多的鱼竿，一次性有很多鱼竿在等，H不断的查看每个鱼竿是否有鱼上钩。增加了效率，减少了等待的时间。
- IO多路转接是多了一个select函数，select函数有一个参数是文件描述符集合，对这些文件描述符进行循环监听，当某个文件描述符就绪时，就对这个文件描述符进行处理。
- IO多路转接是属于阻塞IO，但可以对多个文件描述符进行阻塞监听，所以效率较阻塞IO的高。
![IO多路复用](https://img-blog.csdnimg.cn/20200411182136966.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzEyMjA5MA==,size_16,color_FFFFFF,t_70)


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

> 本篇整理参考自 https://blog.csdn.net/weixin_43122090/article/details/105297786