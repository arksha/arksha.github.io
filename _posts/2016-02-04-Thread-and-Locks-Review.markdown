---
layout: post
title: Thread and Locks Review
date: 2016-02-04
categories: diary
---

##Thread
1.Create thread
	
we have mainly two ways to create thread. First is implement Runnable interface and run() function. Second way is to extend Thread and just call start() on instance class itself.

But we prefer Runnable way because: 

First,Java doesn't support multiple inheritance. A runnable interface can be inherited by other subclass, if extend Thread class, subclass cannot extend other class.

Second, runnable interface means code can be reused by many threads. Code and data is separate, not like extending Thread method.

Third, A class might only interesting with Runnable, so it is excessive to inherit the full Thread class.

2.How to create thread by Runnable Interface

First, create a class to implement Runnable interface.
Second, create a Thread object by passing a runnable object to Thread constructor.Thread has a runnable object that implement run()
Third, use Thread object to call start() on the thread.


##Synchronization and Lock

1.Synchronization

Sync is to control access to shared resources.

**Synchronized keyword**: if same instance-> cannot call funtion in run at the same time but hold different references can call at the same time. Because, Static methods synchronize on the class lock. Two threads with one object could not simulate synchronized static method on the same class.

**Synchronized blocks**: just add ``synchronized(){ }``

2.Lock

Lock is more granular control. Lock(monitor) is useful when resource is accessed in multiple places but should be one thread access at a time. use lock() and unlock() to hold the lock and release it.

3.Deadlocks and Prevention

Waiting for each other finish forever.(Romantic unhuh)

How to prevent: Mutural exclusion, Hold and wait, No preemption: one process cannot forcibly remove  another process' resource, Circular wait.

