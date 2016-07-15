---
title: AsyncTask和Handler-Android
date: 2016-03-27 14:33:45
tags: Android
---
# 背景
* Android多线程同Java多线程概念一致。（进程和线程区别，操作系统学的知识被吃了么少年！）

* Android的主线程为UI线程。
# Handler
> Handler是android 的多线程通信机制的产物,在Android中不允许Activity新启动的线程访问该Activity里的UI组件，这样会导致新启动的线程无法改变UI组件的属性值。但实际开发中，很多地方需要在工作线程中改变UI组件的属性值，比如下载网络图片、动画等等。

Handler，它直接继承自Object，一个Handler允许发送和处理Message或者Runnable对象，并且会关联到主线程的MessageQueue中。每个Handler具有一个单独的线程，并且关联到一个消息队列的线程，就是说一个Handler有一个固有的消息队列。当实例化一个Handler的时候，它就承载在一个线程和消息队列的线程，这个Handler可以把Message或Runnable压入到消息队列，并且从消息队列中取出Message或Runnable，进而操作它们。

Handler可以把一个Message对象或者Runnable对象压入到消息队列中，进而在UI线程中获取Message或者执行Runnable对象，所以Handler把压入消息队列有两大体系，Post和sendMessage:
> 对于Handler的Post方式来说，它会传递一个Runnable对象到消息队列中，在这个Runnable对象中，重写run()方法。一般在这个run()方法中写入需要在UI线程上的操作。hanlder实际上是不涉及多线程的。
* post(Runnable)  让当前线程执行Runnable任务。如果是在主线程调用，那么就是在UI线程执行Runnable。实际上没有多线程执行runnable。
* postAtTime(Runnable,long)  也是让当前线程在 时间点long 执行Runnble
* postDelayed(Runnable long) 让当前线程延时 long 后执行Runnable。
这3个方法实际上可以把handler看成是一个任务调度器，而不是一个多线程相关的。


> Handler如果使用sendMessage的方式把消息入队到消息队列中，需要传递一个Message对象，而在Handler中，需要重写handleMessage()方法，用于获取工作线程传递过来的消息，此方法运行在UI线程上。

Message自带的有如下几个属性：
* int arg1：参数一，用于传递不复杂的数据，复杂数据使用setData()传递。
* int arg2：参数二，用于传递不复杂的数据，复杂数据使用setData()传递。
* Object obj：传递一个任意的对象。
* int what：定义的消息码，一般用于设定消息的标志。
对于Message对象，一般并不推荐直接使用它的构造方法得到，而是建议通过使用Message.obtain()这个静态的方法或者Handler.obtainMessage()获取。Message.obtain()会从消息池中获取一个Message对象，如果消息池中是空的，才会使用构造方法实例化一个新Message，这样有利于消息资源的利用。并不需要担心消息池中的消息过多，它是有上限的，上限为10个。Handler.obtainMessage()具有多个重载方法，如果查看源码，会发现其实Handler.obtainMessage()在内部也是调用的Message.obtain()。
但是Android中，只有主线程才能更改UI中的控件，那么怎么办呢？还好出现了一个类叫Looper类，这个类用于连接，UI线程和Handler之间的关系，当然可以不是UI线程。 
```java
Looper looper = Looper.getMainLooper(); //得到主线程Looper 
Looper looper = Looper.myLooper();//得到当前线程的Looper 
```
摘自[[Android--多线程之Handler](http://www.cnblogs.com/plokmju/p/android_handler.html)]


