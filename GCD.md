#GCD
##简介
1.什么是GCD   

- 纯C语言，提供了非常多强大的函数


	- GCD会自动将队列中的任务取出，放到对应的线程中执行
1.执行任务

- 同步

		dispatch_sync(dispatch_queue_t queue, dispatch_block_t block);

		dispatch_async(dispatch_queue_t queue, dispatch_block_t block);

- 并发队列（Concurrent Dispatch Queue）

3.同步、异步、并发、串行
	
##创建队列
1.并发队列

- GCD默认已经提供了全局的并发队列，供整个应用使用，不需要手动创建

dispatch_queue_t dispatch_get_global_queue(
```
- 全局并发队列的优先级

```objc
```
2.串行队列

- 使用dispatch_queue_create函数创建串行队列

dispatch_queue_t
```
- 使用主队列（跟主线程相关联的队列）
	
	dispatch_queue_t queue = dispatch_get_main_queue();
	```

3.各种队列的执行效果

![](./各队列执行效果.png)

- 使用**sync**函数往**当前串行**队列中添加任务，会卡住当前的串行队列