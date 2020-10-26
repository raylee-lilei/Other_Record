---
title: CPPInterview
author: raylee
avatar: 'https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/custom/logo_1.png'
authorLink: 
authorAbout: industry pays
authorDesc: industry pays
categories: technology
comments: true
date: 2020-03-30 10:44:57
tags:
keywords: C++ 面试
description:
photos: https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.0/img/cover/03.jpg.webp
---

分享别人的C++面试问题


## gdb调试命令

### step和next的区别
当前line有函数调用的时候,next会直接执行到下一句（F11）,step会进入函数（F10）。

### 查看内存

``` bash
$(gdb)p &a //打印变量地址

$(gdb)x 0xbffff543 //查看内存单元内变量

$0xbffff543: 0x12345678

$(gdb)x/4xb 0xbffff543 //单字节查看4个内存单元变量的值

$0xbffff543: 0x78 0x56 0x34 0x12
}  
```

### 多线程调试
``` bash
$(gdb) info threads：查看GDB当前调试的程序的各个线程的相关信息

$(gdb) thread threadno：切换当前线程到由threadno指定的线程

$break filename:linenum thread all   在所有线程相应行设置断点，注意如果主线程不会执行到该行，并且启动all-stop模式，主线程执行n或s会切换过去

$set scheduler-locking off|on\step  默认off，执行s或c其它线程也同步执行。on，只有当前线程执行。step，只有当前线程执行

$show scheduler-locking     显示当前模式

$thread apply all command   每个线程执行同意命令，如bt。或者thread apply 1 3 bt，即线程1，3执行bt。 
```
### 查看调用堆栈

``` bash
$(gdb)bt

$(gdb)f 1 帧简略信息

$(gdb)info f 1 帧详细信息  
```

### 断点

```bash
b test.cpp:11
b test.cpp:main

gdb attach 调试方法：
gdb->file xxxx->attach pid->这时候进程是停止的->c 继续运行
```

### 带参数调试
输入参数命令set args 后面加上程序所要用的参数，注意，不再带有程序名，直接加参数，如：
```bash
(gdb)set args -l a -C abc
```

### list命令
```bash
list　linenum　　显示程序第linenum行的周围的程序
list　function　　显示程序名为function的函数的源程序
```

## 软硬链接

```bash
ln -s 源文件 目标文件, ln -s / /home/good/linkname链接根目录/到/home/good/linkname
```
1、软链接就是：“ln –s 源文件 目标文件”，只会在选定的位置上生成一个文件的镜像，**不会占用磁盘空间**，类似于windows的快捷方式。
2、硬链接ln源文件目标文件，**没有参数-s**， 会在选定的位置上生成一个和源文件**大小相同**的文件，无论是软链接还是硬链接，文件都保持同步变化。

## 函数指针

函数指针 int (*func)(int, int)

函数指针数组 int (*funcArry[10])(int, int)

const int* p; 指向const int的指针

int* const p; const指针

## 设计模式

### 单例模式
**一对一**
● 1、单例类只能有一个实例。
● 2、单例类必须自己创建自己的唯一实例。
● 3、单例类必须给所有其他对象提供这一实例。
比如：
● 1、Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。
● 2、一些设备管理器常常设计为单例模式，比如一个电脑有两台打印机，在输出的时候就要处理不能两台打印机打印同一个文件。

### 观察者模式
**一对多**
● (也叫发布订阅模式)当一个对象被修改时，则会自动通知它的依赖对象

### 工厂模式 
三种：简单工厂模式、工厂方法模式、抽象工厂模式

### 为什么要用工厂模式？
原因就是**对上层的使用者隔离对象创建的过程**；或者是对象创建的过程复杂，使用者不容易掌握；或者是对象创建要满足某种条件，这些条件是业务的需求也好，是系统约束也好，没有必要让上层使用者掌握，增加别人开发的难度。所以，到这时我们应该清楚了，无论是工厂模式，还是开闭原则，都是为了隔离一些复杂的过程，使得这些**复杂的过程不向外暴露**，如果暴露了这些过程，会对使用者增加麻烦，这也就是所谓的**团队合作**。

### 大小端转化
```bash

#define BigLittleSwap32(A) 
					((((uint32)(A) & 0xff000000) >> 24)| \
                    (((uint32)(A) & 0x00ff0000) >> 8) | \
                    (((uint32)(A) & 0x0000ff00) << 8) | \
                    (((uint32)(A) & 0x000000ff) << 24))
```

### IO多路复用
#### 为什么 IO 多路复用要搭配非阻塞IO
**select和read两个操作相互独立且存在窗口，select返回可读并不能保证read一定可读，存在多种情况，select返回可读，但是read无数据可读**
● 多进程同时对某个socket进行监听，当新的连接完成3次握手后，进程均被select,epoll唤醒，但是最后只有1个进程可以accept，没能accept的进程被block
● 某个socket接收缓冲区有新数据分节到达，然后select报告这个socket描述符可读，但随后，协议栈检查到这个新分节检验和错误，然后丢弃这个分节，这时候调用read则无数据可读
● 边缘触发环境，由于无法知道多少数据可读，所以accept1次后，第二次尝试accept可能会被阻塞，此时应该使用非阻塞IO

设置非阻塞 
```bash
io fcntl(sockfd, F_SETFL, O_NONBLOCK);
```

