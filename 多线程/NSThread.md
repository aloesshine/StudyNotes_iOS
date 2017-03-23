
# NSThread

## 基本使用


1.创建和启动线程

```objc
NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(run) object:nil];[thread start];// 线程一启动，就会在线程thread中执行self的run方法// 主线程相关用法+ (NSThread *)mainThread; // 获得主线程- (BOOL)isMainThread; // 是否为主线程+ (BOOL)isMainThread; // 是否为主线程
```
2.其他用法

``` objc// 获得当前线程NSThread *current = [NSThread currentThread];// 线程的名字- (void)setName:(NSString *)n;- (NSString *)name;
```
3.其他创建线程方式

```objc
// 创建线程后自动启动线程[NSThread detachNewThreadSelector:@selector(run) toTarget:self withObject:nil];// 隐式创建并启动线程[self performSelectorInBackground:@selector(run) withObject:nil];
```
- 优点：简单快捷- 缺点：无法对线程进行更详细的设置

## 线程的状态

- **New（新建状态）** 执行 - start 方法 ---> **Runnable（就绪状态）**
- **Runnable（就绪状态）** CPU调度当前线程 ---> **Running（运行状态）** ,调度其他线程 ---> **Runnable（就绪状态）**
- **Running（运行状态）** 时调用sleep方法/等待同步锁 ---> **Blocked（阻塞状态）**，sleep到时/得到同步锁 ---> **Runnable（就绪状态）**
- **Running（运行状态）** 时线程任务执行完毕异常/强制退出 ---> **Dead（死亡状态）**

*注意：一旦线程停止（死亡）了，就不能再次开启任务*

```objc
启动线程- (void)start; // 进入就绪状态 -> 运行状态。当线程任务执行完毕，自动进入死亡状态阻塞（暂停）线程+ (void)sleepUntilDate:(NSDate *)date;+ (void)sleepForTimeInterval:(NSTimeInterval)ti;// 进入阻塞状态强制停止线程+ (void)exit;// 进入死亡状态```

## 多线程的安全隐患

1.多个线程可能会访问同一块资源
	- 容易引发**数据错乱**和**数据安全**问题

2.隐患解决 ---- **互斥锁**

-  使用格式
	
	```objc
	@synchronized(锁对象) { // 需要锁定的代码  }
	```	*注意：锁定1份代码只用1把锁，用多把锁是无效的*
	   
- 优缺点	- 优点：能有效防止因多线程抢夺资源造成的数据安全问题	- 缺点：需要消耗大量的CPU资源	
- 使用前提：多条线程抢夺同一块资源- **线程同步**
	- 多条线程在同一条线上执行（按顺序地执行任务）	- 互斥锁，就是使用了线程同步技术
	## 原子和非原子属性

1.atomic

- 原子属性，为setter方法加锁（默认就是atomic）
- 线程安全，需要消耗大量的资源
2.nonatomic

- 非原子属性，不会为setter方法加锁- 非线程安全，适合内存小的移动设备3.iOS开发的建议
- 所有属性都声明为nonatomic- 尽量避免多线程抢夺同一块资源- 尽量将加锁、资源抢夺的业务逻辑交给服务器端处理，减小移动客户端的压力

## 线程间通信

1.体现

- 1个线程传递数据给另1个线程- 在1个线程中执行完特定任务后，转到另1个线程继续执行任务

2.线程间通信常用方法

```objc- (void)performSelectorOnMainThread:(SEL)aSelector withObject:(id)arg waitUntilDone:(BOOL)wait;- (void)performSelector:(SEL)aSelector onThread:(NSThread *)thr withObject:(id)arg waitUntilDone:(BOOL)wait;
```
3.利用NSPort：复杂，不推荐 
