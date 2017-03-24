# RunLoop

## 简介

1.什么是RunLoop

- 简单理解，运行循环

2.基本作用
- 保持程序的持续运行- 处理App中的各种事件（比如触摸事件、定时器事件、Selector事件）- 节省CPU资源，提高程序性能：该做事时做事，该休息时休息3.main函数中的RunLoop

- UIApplicationMain函数内部就启动了一个RunLoop- 所以UIApplicationMain函数一直没有返回，保持了程序的持续运行- 这个默认启动的RunLoop是跟**主线程**相关联的## RunLoop对象

1.iOS中有2套API来访问和使用RunLoop- Foundation ： NSRunLoop- Core Foundation ： CFRunLoopRef （开源）> NSRunLoop和CFRunLoopRef都代表着RunLoop对象   > NSRunLoop是基于CFRunLoopRef的一层OC包装

2.RunLoop与线程

- 每条**线程**都有**唯一**的一个与之对应的**RunLoop对象**- 主线程的RunLoop已经自动创建好了，**子线程**的RunLoop需要**主动创建**- RunLoop在第一次获取时创建（懒加载），在**线程结束时销毁**

3.获得RunLoop对象

```objc
// Foundation[NSRunLoop currentRunLoop]; // 获得当前线程的RunLoop对象[NSRunLoop mainRunLoop]; // 获得主线程的RunLoop对象// Core FoundationCFRunLoopGetCurrent(); // 获得当前线程的RunLoop对象CFRunLoopGetMain(); // 获得主线程的RunLoop对象
```## RunLoop相关类

1.简介

- Core Foundation中关于RunLoop的5个类	- CFRunLoopRef	- CFRunLoopModeRef	- CFRunLoopSourceRef	- CFRunLoopTimerRef	- CFRunLoopObserverRef

	<img src="./RunLoop.png" width="300"/>
	
2.CFRunLoopModeRef

- CFRunLoopModeRef 代表 RunLoop 的**运行模式**	- 一个 RunLoop 包含若干个 Mode，每个Mode又包含若干个Source/Timer/Observer	- 每次 RunLoop 启动时，只能指定其中一个 Mode，这个 Mode 被称作 CurrentMode	- 如果需要切换 Mode，只能**退出Loop，再重新指定一个Mode**进入	- 这样做主要是为了**分隔开不同组的Source/Timer/Observer**，让其互不影响
	
- 系统默认注册了5个Mode:	- **kCFRunLoopDefaultMode**：App的默认Mode，通常主线程是在这个Mode下运行	- **UITrackingRunLoopMode**：界面跟踪 Mode，用于 ScrollView 追踪触摸滑动，保证界面滑动时不受其他 Mode 影响	- UIInitializationRunLoopMode: 在刚启动 App 时第进入的第一个 Mode，启动完成后就不再使用	- GSEventReceiveRunLoopMode: 接受系统事件的内部 Mode，通常用不到	- kCFRunLoopCommonModes: 这是一个占位用的Mode，不是一种真正的Mode
