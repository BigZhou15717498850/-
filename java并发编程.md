#Java并发编程
## 1. java内存模型
在并发编程中，需要处理两个关键问题，线程之间如何通信及线程之间如何同步

线程之间的通信机制有两种

* 共享内存
* 消息传递

并发执行的三个重要性质

* 原子性
* 可见性
* 有序性

并发编程的等待/通知经典范式

两个部分：消费者（等待）和生产者（生产->通知）

1. 获取对象锁
2. 检查条件，不满足则等待，唤醒之后继续检查
3. 条件满足则执行相应逻辑

	* 消费者

			synchronized(lock){
				while(条件不满足){
					lock.wait();
				}
			}
	* 生产者
			
			synchronized(lock){
				改变条件；
				lock.notify();
			}

