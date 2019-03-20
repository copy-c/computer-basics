# 第一章 线程安全的对象生命期管理
## 存在问题
1.对象中的mutex成员只能保证类内数据的原子性，不能保证对象正确销毁  
2.交换时死锁——swap(a,b) swap(b,a) 同时执行  
```c
void swap(Counter &a, Counter &b)
{
    MutexLockGuard aLock(a.mutex_);
    MutexLockGuard bLock(b.mutex_);
    /// ...
}
```
3.动态创建的对象通过指针访问，无论内存上的对象是否被销毁，单从指针上并得不到答案  
## 解决方案 —— 智能指针（shared_prt/weak_ptr）  
1.简介  
shared_ptr：引用计数型 —— 可能产生循环引用  
weak_ptr::引用计数型，但是不增加对象的引用次数，像一个助手  
    .lock() // 返回一个shared_ptr 若对象已经释放则为空  
    .expired() // 检测对象是否已经被释放  
    构造shared_ptr()  
2.本质：  
使用栈上RAII技术来管理堆上资源（通过主动调用delete）  

# 第二章 线程同步精要
## 互斥器（mutex）——加锁原语
1.使用原则  
1）用RAII手法封装mutex的创建、销毁、加锁、解锁四个操作 —— 可以保证加解锁在同一个线程、不会忘记解锁、Guard是栈上对象，看调用栈就可以分析锁情况  
2）只是用非递归mutex（不可重入锁）  也即同一个线程不能重复对一个线程加锁
3）不手工调用lock()和unlock()  
4）不使用跨进程mutex，进程通信只使用TCP  
2.影响性能  
1）false sharing 伪共享  
## 条件变量 （condition variable）—— 等待原语
1.java object 内置wait() notify() notifyAll()  
2.BlockingQueue  
```c
mudou::MutexLock mutex;
mudou::Condition cond(mutex);
std::deque<int> queue;

// wait端
// wait必须与mutex一起使用，布尔表达式的保护需要mutex
int dequeue()
{
    MutexLockGuard lock(mutex);
    while (queue.empty()) // 必须使用循环，不能使用if判断，因为wait可能会被虚假唤醒 spurious wakeup
    {
        cond.wait(); // 这一步会原子 unlock mutex 并进入等待，不会与入队列死锁
        // wait() 执行完毕后会自动重新加锁
    }

    int top = queue.front();
    queue.pop_front();
    return top;
}

// signal/broadcast端
void enqueue(int x)
{
    MutexLockGuard lock(mutex);
    queue.push_back();
    cond.notify();
}
```
## 使用pThread实现锁、卫兵锁、条件变量
```c
class MutexLock : boost::noncopyable
{
public:
    MutexLock() : holder_(0) 
    {
        pthread_mutex_init(&mutex_, NULL);
    }

    ~MutexLock()
    {
        assert(holder_ == 0);
        pthread_mutex_destory(&mutex_);
    }

    bool isLockedByThisThread()
    {
        return holder_ == CurrentThread::tid();
    }

    friend class MutexLockGuard;

private:
    void lock()
    {
        pthread_mutex_lock(&mutex_);
        holder_ = CurrentThread::tid();
    }

    void unlock()
    {
        holder_ = 0;
        pthread_mutex_unlock(&mutex_);
    }

    pthread_mutex_t* getPthreadMutex()
    {
        return &mutex_;
    }

private:
    pthread_mutex_t mutex_;
    pid_t holder_; 
};

class MutexLockGuard : boot::noncopyable
{
public::
    explicit MutexLockGuard(MutexLock& mutex) : mutex_(mutex)
    {
        mutex_.lock();
    }

    ~MutexLockGuard()
    {
        mutex_.unlock();
    }
};
```
# 第三章 多线程服务器的适应场景与常用编程模型
## 单线程常用编程模型
1.Reactor：non-blocking io + IO multiplexing  
程序基本结构为：事件循环 + 事件驱动 + 事件回调  
```c
	while (!done)
	{
		int timeout_ms = max(1000, getNextTimeCallback());
		int retval = ::poll(fds, nfds, timeout_ms);
		if (retval < 0)
		{
			// 处理错误，回调用户的error handler
		}
		else
		{
			// 处理到期的 timers，回调用户的timer handler
			if (retval > 0)
			{
				// 处理IO事件，回调用户的IO event handle
			}
		}
	}
```
## 多线程常用编程模型
1.Reactor  
2.线程池  
使用任务队列实现 或 BlockingQueue  
3.进程间通信只用TCP

## 线程分类
1.IO线程  
主循环是IO multiplexing，阻塞等在select/poll/epoll_wait系统调用上  
2.计算线程  
主循环是blocking queue，阻塞等在condtition variable上，一般位于thread pool，尽量避免阻塞  
3、第三方库线程  
logging、database connection  

# 第四章 多线程系统精要
## 基本线程原语选用
Liunx平台下使用thread、mutex、condition即可完成多线程编程任务  

# 第六章 muduo网络库
1.SocketApi   
```c
服务端                      客户端
bind
listing
accept //等待客户端连接       connect //连接服务端
recv         		    recv
send                        send
close                       close
```
2.TCP网络编程本质 —— 三个半事件  
 - 连接的建立：服务端accpet && 客户端connect，一旦建立，两方都是平等的recv和send    
 - 连接的断开：主动的断开和被动的断开  
 - 消息到达，文件描述符处理
 - 消息发送完毕

3.**线程模型等核心问题**  
 - even loop是non-blocking网络编程的核心，且和IO multiplexing（实质为当前线程的复用，让IO阻塞在系统自己的调用上）一起使用。类似窗口程序不能阻塞窗口  
 - even loop + non-blocking 必须使用buffer，output buffer不能在write上阻塞，直接send继续执行；input buffer收到的数据尚不够构成一条完整消息，等完整消息后再通知业务逻辑    
 - task thread <----> buffer <----> even loop + non-blocking + level trigger
 

# 第七章 muduo编程实例
## 简单TCP实例
1.time客户端  
非阻塞的网络编程必须使用接收缓存，需要等待接收数据达到目标大小才可以处理  
```c
if (buf->readableBytes() >= sizeof(int32_t))
{
    const char* data = buf->peek(); 
}
```
2.流量统计  
```c
void ChargenServer::onWriteComplete(const TcpConnectionPtr& conn)
{
  transferred_ += message_.size();
  conn->send(message_);
}
void ChargenServer::printThroughput()
{
  Timestamp endTime = Timestamp::now();
  double time = timeDifference(endTime, startTime_);
  printf("%4.3f MiB/s\n", static_cast<double>(transferred_)/time/1024/1024); // message的size就是char的大小，就是一个字节大小
  transferred_ = 0;
  startTime_ = endTime;
}
```
## Buffer类的设计与使用


# 第九章 分布式系统
## 分布式系统中的心跳协议的设计
1.Tcp keepalive不能替代应用层心跳  
原因：Tcp keepalive由操作系统探测，即使进程死锁或者阻塞，操作系统也会如常收发TCP keepalive消息  
2.两个关键点  
1）在工作线程发送，不能单独起一个心跳线程 —— 防止工作线程死锁或者阻塞后还继续发心跳  
2）与业务消息用同一个连接，不能单独使用心跳连接
## 分布式系统部署、监控与进程管理
1.分布式系统中服务程序的依赖关系  
被依赖：可以通过TCP通信，通过netstat就可以知道被依赖的程序  

# 第十章 C++编译链接模型
## C语言编译的模型及其成因
1.#include的缺点：会把所有头文件全部展开，导致文件过大  
## C++编译模型 
1.单编编译  
只能看到之前定义的，若存在多个同名函数，只能在调用处之前寻找最佳的定义  
2.前向声明  
通过前向声明可以不使用#include，进而省下文件大小  

