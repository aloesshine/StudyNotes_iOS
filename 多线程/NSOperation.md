# NSOperation

## 基本概念

1.作用
- 配合使用 NSOperation 和 NSOperationQueue 也能实现多线程编程2.实现多线程的具体步骤- 先将需要执行的操作封装到一个NSOperation对象中- 然后将NSOperation对象添加到NSOperationQueue中- 系统会自动将NSOperationQueue中的NSOperation取出来- 将取出的NSOperation封装的操作放到一条新线程中执行

3.NSOperation的子类- NSOperation是个**抽象类**，并不具备封装操作的能力，必须使用它的子类- 方式
	- NSInvocationOperation	- NSBlockOperation	- 自定义子类继承NSOperation，实现内部相应的方法## 具体使用

1.NSInvocationOperation

- 创建NSInvocationOperation对象

	```objc	- (id)initWithTarget:(id)target selector:(SEL)sel object:(id)arg;	```
	
- 调用start方法开始执行操作	```objc	- (void)start;	//一旦执行操作，就会调用target的sel方法	```
	
- 默认情况下，调用了start方法后并不会开一条新线程去执行操作，而是在**当前线程同步**执行操作- 只有将NSOperation放到一个NSOperationQueue中，才会异步执行操作2.NSBlockOperation

- 创建NSBlockOperation对象

	```objc	+ (id)blockOperationWithBlock:(void (^)(void))block;	```
	
- 通过addExecutionBlock:方法添加更多的操作

	```objc	- (void)addExecutionBlock:(void (^)(void))block;	```
	
- 只要NSBlockOperation封装的操作数 > 1，就会**异步**执行操作## NSOperationQueue

1.作用
- NSOperation可以调用**start**方法来执行任务，但默认是**同步**执行的- 如果将NSOperation添加到NSOperationQueue（操作队列）中，系统会**自动异步执行**NSOperation中的操作

2.添加操作到NSOperationQueue中

```objc- (void)addOperation:(NSOperation *)op;- (void)addOperationWithBlock:(void (^)(void))block;```3.最大并发数的相关方法

```objc- (NSInteger)maxConcurrentOperationCount;- (void)setMaxConcurrentOperationCount:(NSInteger)cnt;```

- 并发数: 同时执行的任务数
	- 设为1，变成串行队列

4.队列的取消、暂停、恢复

```objc
// 取消队列的所有操作- (void)cancelAllOperations;// 提示：也可以调用NSOperation的- (void)cancel方法取消单个操作// 暂停和恢复队列- (void)setSuspended:(BOOL)b; // YES代表暂停队列，NO代表恢复队列- (BOOL)isSuspended;
``` - 串行队列时，取消/挂起之后，**不会立即停止操作**，而是等当前任务执行完毕，停止执行之后的任务## NSOperation其他用法

1.操作依赖

- NSOperation之间可以设置依赖来保证执行顺序	
	```objc	[operationB addDependency:operationA]; // 操作B依赖于操作A	```
	
- 可以在不同queue的NSOperation之间创建依赖关系- 两个操作不能相互依赖

2.操作的监听

```objc
// 可以监听一个操作的执行完毕- (void (^)(void))completionBlock;- (void)setCompletionBlock:(void (^)(void))block;```

3.自定义NSOperation

- 重写- (void)main方法，在里面实现想执行的任务	- 自己创建自动释放池（因为如果是异步操作，无法访问主线程的自动释放池）	- 经常通过- (BOOL)isCancelled方法检测操作是否被取消，对取消做出响应4.线程间通信

```objc
[[[NSOperationQueue alloc] init] addOperationWithBlock:^{
        // 子线程操作...
        
        // 回到主线程
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            
        }];
    }];

```

## GCD与NSOperation的选择

- GCD是底层的C语言构成的API，而NSOperationQueue及相关对象是Objc的对象。在GCD中，在队列中执行的是由block构成的任务，这是一个轻量级的数据结构；而Operation作为一个对象，为我们提供了更多的选择；
- 在NSOperationQueue中，我们可以随时取消已经设定要准备执行的任务(当然，已经开始的任务就无法阻止了)，而GCD没法停止已经加入queue的block(其实是有的，但需要许多复杂的代码)；
- NSOperation能够方便地设置依赖关系，我们可以让一个Operation依赖于另一个Operation，这样的话尽管两个Operation处于同一个并行队列中，但前者会直到后者执行完毕后再执行；
- 我们能将KVO应用在NSOperation中，可以监听一个Operation是否完成或取消，这样子能比GCD更加有效地掌控我们执行的后台任务；
- 在NSOperation中，我们能够设置NSOperation的priority优先级，能够使同一个并行队列中的任务区分先后地执行，而在GCD中，我们只能区分不同任务队列的优先级，如果要区分block任务的优先级，也需要大量的复杂代码；
- 我们能够对NSOperation进行继承，在这之上添加成员变量与成员方法，提高整个代码的复用度，这比简单地将block任务排入执行队列更有自由度，能够在其之上添加更多自定制的功能。