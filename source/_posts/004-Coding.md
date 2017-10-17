---
title: 004_Coding
date: 2017-08-09 15:26:22
tags: open source 
---


# guava 

EventBus

.register(XXListener{})
.post(event)

AllowConcurrentEvents


PreCondition 

ListenableFuture可以允许你注册回调方法(callbacks)，在运算（多线程执行）完成的时候进行调用, 或者在运算（多线程执行）完成后立即执行。这样简单的改进，使得可以明显的支持更多的操作，这样的功能在JDK concurrent中的Future是不支持的。

MoreExecutors.listeningDecorator
MoreExecutors.addDelayedShutdownHook

# lombok



# metrics (dropwizard)



