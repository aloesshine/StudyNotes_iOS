
#NSThread
##基本使用

1.创建和启动线程

```objc
NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(run) object:nil];
 (NSThread *)mainThread; // 获得主线程
 (BOOL)isMainThread; // 是否为主线程
```
2.其他用法

``` objc
```
3.其他创建线程方式

```objc
// 创建线程后自动启动线程
```
 	- 优点：简单快捷

##线程的状态

- **New（新建状态）** 执行 - start 方法 ---> **Runnable（就绪状态）**
- **Runnable（就绪状态）** CPU调度当前线程 ---> **Running（运行状态）** ,调度其他线程 ---> **Runnable（就绪状态）**
- **Running（运行状态）** 时调用sleep方法/等待同步锁 ---> **Blocked（阻塞状态）**，sleep到时/得到同步锁 ---> **Runnable（就绪状态）**
- **Running（运行状态）** 时线程任务执行完毕异常/强制退出 ---> **Dead（死亡状态）**



```
启动线程
 (void)sleepUntilDate:(NSDate *)date;
 (void)sleepForTimeInterval:(NSTimeInterval)ti;
 (void)exit;

##多线程的安全隐患

1.多个线程可能会访问同一块资源
	- 容易引发**数据错乱**和**数据安全**问题

2.隐患解决 ---- **互斥锁**

-  使用格式
	
	```
	@synchronized(锁对象) { // 需要锁定的代码  }
	```
	   
- 优缺点
- 使用前提：多条线程抢夺同一块资源
	- 多条线程在同一条线上执行（按顺序地执行任务）
	

1.atomic

- 原子属性，为setter方法加锁（默认就是atomic）
- 线程安全，需要消耗大量的资源


- 非原子属性，不会为setter方法加锁


##线程间通信

1.体现

- 1个线程传递数据给另1个线程

2.线程间通信常用方法

```
```
3.利用NSPort：复杂，不推荐
