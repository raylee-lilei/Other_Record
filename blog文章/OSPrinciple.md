---
title: OSPrinciple
author: raylee
avatar: 'https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/custom/logo_1.png'
authorLink: 
authorAbout: industry pays
authorDesc: industry pays
categories: technology
comments: true
date: 2020-04-02 15:30:09
tags:
keywords: OS Principle
description: 
photos: https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/os.jpg
---

操作系统原理分析

## 操作系统功能

### 1.进程管理（CPU管理）
进程控制：创建，暂停，唤醒，撤销
进程调度：调度策略，优先级
进程通信
### 2.内存管理
内存分配，共享，保护，虚拟内存

### 3.设备管理
设备的分配，调度，设备传输，设备驱动
### 4.文件管理
存储空间，文件目录的操作，存取权限的管理

## 分时技术（Unix）
交互性，响应性
多终端计算机（一个主机下面有很多终端，共享主机的CPU资源，主机以很短的时间片为单位，把CPU轮流的分配给终端使用，直到全部作业运行完）
多路调制性（多用户联机使用同一台计算机）
独占性（每个终端感觉自己独占CPU）
交互性（及时响应用户请求）

## OS的逻辑结构
### 整体式结构
以模块化，编码调试相对独立，调用自由，同行以全局变量形式完成
### 层次式结构
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/2.png " width = 45% height = 30% />
相邻层只有单向依赖或者单向调用（结构清晰 有利于移植，维护，扩充）
### 微内核结构
微内核（最核心的服务）+核外服务器（进程，设备管理服务器等）

  **CPU态：**（对资源和使用权限的描述）
  **核态：**能够访问所有资源额执行的所有指令；管理程序OS内核
  **用户态：**访问资源受限。用户程序

在硬件层是以“态”区分
在CPU层是以“进程”区分

## 存储器
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/3.png " width = 55% height = 40% />

## 中断
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/4.png" width = 60% height = 50% />

**引入中断的目的：**
1.实行并发活动（CPU和外设）
2.实行实时处理
3.故障自动处理

**中断响应过程：**
1.识别中断源
2.保护断点和线程
3.装入中断服务程序的入口地址（cs:ip）
4.进入中断程序
5.恢复现场和断点
6.中断返回（IRET）        

**中断的实质：**
交换指令执行地址
交换CPU的态  

## 工作模式
### 实模式  
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/5.png" width = 55% height = 45% />
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/6.jpg" width = 55% height = 45% />
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/7.png" width = 55% height = 45% />

### 保护模式
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/8.png" width = 55% height = 45% />

## BIOS功能
### 系统启动配置
基本的设备I/O服务
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/9.png" width = 53% height = 45% />
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/10.png" width = 53% height = 45% />
系统的加电自检和启动          

按下电源键计算机执行第一条指令是FFFF0的指令(JUMP POST )——>加电自检 (POST )——>初始化CPU、内存、显卡(查找显卡BIOS，调用显卡BIOS，一次查找其他设备的BIOS)——>显示启动画面——>从硬盘、U盘、光驱等读取OS——>OS 启动，OS接管计算机

### 启动过程
1.初始引导（BIOS启动程序，加载MBR的引导程序—GRUB，加载内核OS）
2.核心初始化
3.系统初始化

**主启动记录（MBR）** 
存放在硬盘的首扇区，存放OS启动相关信息，512 bytes,结束：0XAA55h
每个分区的首扇区记录主启动记录；
主启动扇区：提供菜单（选择不同的OS）；
加载核心文件（指向可启动的OS）；
跳转（将启动功能交给其他的loader）   

**BIOS和MBR程序运行过程：**		
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/11.jpg" width = 53% height = 45% />  
         
## 用户界面
### 1.操作界面
图形用户接口；键盘命令：普通命令，批处理程序，shell

### 2.系统调用   有唯一的ID
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/12.jpg" width = 53% height = 40% />  
           
# 进程管理
## 进程状态
运行态:进程占有CPU
就绪态：具备运行条件无CPU，暂时不能运行
阻塞态：等待某项服务完成或者信号不能运行，等待外设完成相应的I/O操作，等待其他进程给他提供信号
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/13.jpg" width = 53% height = 40% />  

### Linux进程的状态
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/14.png" width = 53% height = 40% />  

进程控制块PCB是一种描述进程状态，资源，相关进程的数据结构
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/15.jpg" width = 38% height = 25% />  
主线程是缺省出来的

<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/16.png" width = 53% height = 40% />  

忙则等待：当临界区忙，其他进程必须在临界区外等待
空闲让进：当没有进程在临界区，任何进程有权进入临界区
有限等待：待进入临界区的进程应该有限时间内得到满足
让权等待：当在等待进入临界区的进程放弃CPU
临界区越小越好
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/17.png" width = 43% height = 30% />  

互斥：多个进程共享了独占性资源，确保没有任何两个以上的进程同时进行存取操作
同步：合作进程需要满足先后关系，一个进程能否进行取决于某个前提条件，否则只能等待（公交车和售票员）

## 信号灯
进程在运行过程中受信号灯状态的控制，并能改变信号灯状态
### P操作
用二元矢量表示（S,q） S整数，初始非负，信号量   q  PCB队列
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/18.png" width = 53% height = 40% />  
P操作可能会使进程在调用出阻塞  S初始值很重要
### V操作
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/19.png" width = 53% height = 40% />  
V操作可能会唤醒阻塞的进程

#### P-V操作解决互斥问题
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/20.png" width = 53% height = 40% />  

#### Q-V操作解决同步问题
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/21.png" width = 53% height = 40% />  

#### 生产者消费者同步控制问题
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/22.jpg" width = 58% height = 45% />  

### Windows同步机制
#### 临界区机制
（CREARTE_SECTION）：临界资源的访问（允许一个）
EnterCriticalSection
LeaveCriticalSection
#### 互斥量机制
跨进程使用（允许一个）
CreateMutex
#### 信号量机制
允许多个线程/进程访问临界区 并发访问资源
WaitForSingleObject()减一
ReleaseSemaphore()加一
信号量的值大于0  有信号     小于等于0   无信号
#### 事件机制

### Linux父子进程
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/23.png" width = 53% height = 40% />  

父进程等待——>子进程——>exit()——>父进程善后子进程,收回资源，撤回PCB信息。pid_2获取的是子进程的pid_1——>exit()

父子进程共享普通变量 
int i = 1   即使先运行子进程i= 2，在运行父进程i,他们互不影响。输出子进程i =2;父进程i=1;
父子进程共享文件资源
父子进程共享同一文件和读写指针。子进程写入，父进程写入 ，最终会一起写入

### Windows匿名管道通信
仅能用于父子或者兄弟进程通信
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/24.png" width = 53% height = 40% />  
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/25.png" width = 53% height = 40% />  

1.父进程创建两个管道CreatePipe
2.父进程创建子进程CreateProcess
3.父进程写入管道1
4.父进程读取管道2

### Linux信号通信机制
信号的来源
1.键盘输入组合键
2.终端命令
3.调用函数
4.硬件或者内核异常
Linux定义了64 中信号编号1——64
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/26.png" width = 48% height = 35% />  

父进程fork子进程，发送（kill）自定义信号SIGUSR1，子进程注册信号signal指向handler处理相应要求，exit，然后执行父进程

## 死锁
两个或者多个进程无限期的等待永远不会发生的条件的状态
结果没个进程都陷入阻塞

### 死锁的原因
1.系统资源有限,但是系统资源有限不一定会发生死锁	
2.并发进程推进顺序不当  进程请求释放资源顺序不当
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/27.png" width = 53% height = 40% />  

假设A先运行 当运行到第5行时，运行B，此时j 被占用B在第二行阻塞，然后返回A知道第7行之后被释放继续运行B，并没有导致死锁
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/28.png" width = 53% height = 40% />  

假设A先运行，当运行到第4行时，运行B，B在第4行j 被占用阻塞，使用i，i被占用进程A也被阻塞，导致死锁

3.不正确的P-V操作也会带来死锁（消费者和生产者例子的P-V顺序使用不当）

### 死锁的必要条件
1.互斥条件：进程互斥使用资源，资源具有独占性
2.不剥夺条件：进程访问资源前，不被其他进程强行剥夺	
3.部分分配条件：临时需要临时分配
4.环路条件
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/29.png" width = 53% height = 40% />  

### 解决死锁的策略
**1.预防死锁**
	通过设置某些条件，破坏死锁的必要条件
	由于限制太严格，资源利用率低
**2.避免死锁**
	若分配资源是否会让系统进入死锁状态，是，拒绝分配资源。很难实现
**3.检测恢复死锁**
	先检测恢发生死锁，然后清除死锁	  实现难度大
**4.预先静态分配法**
	破坏部分分配条件：运行进程前，将所需资源一次性全部分配
	开销大，利用率低，延迟
**5.有序资源分配**
	破坏环路条件
	给每个资源分配一个序号；进程每次申请资源时，只能申请序号更大的资源

●按有序资源分配法分配资源并发进程不会发生死锁（序号递增）
●操作系统没有死锁解决方案，留给用户解决


## 进程调度算法
**1.先来先服务**
只考虑时间，不利于短作业
**2.短作业优先调度算法**
不利于长作业，易出现饥饿现象
**3.响应比高优先调度算法**
响应比=1+等待时间/运行时间，有利于短作业或者长作业（兼顾）
**4.优先数调度算法**
优先数=静态优先数+动态优先数
静态：进程所需资源；运行时间长短；进程的类型（IO/CPU，前台/后台，核心/用户）
动态：使用CPU超过一定时长时（大进程降低优先数）；当进行I/O操作后（增加优先数）；当进程等待时间超过一定时间后（增加优先数）
**5.循环轮转调度算法**
先进先出排成队列，进程以时间片段q轮流使用CPU，队列逻辑上形成环形
公平性：每个进程都平等的有机会获得CPU
交互性：每个进程等待（N-1）*q的时间可以重新获得CPU
若q太大相当于先来先服务
若q太小进程切换频繁，系统开销大

## Linux进程调度
**普通进程：**采用动态优先级。进程占用CPU，优先级随时间流失减小 task_struct 的counter表示动态优先级
**实时进程：**静态优先级。 进程创建时指定或者优用户修改  先进先出 时间片轮转

新建子进程counter从父进程时间片counter中继承一半

### 进程调度
不同的进程有不同的调度需求（linux中进程优先级是动态的）
进程调度时机（schedule函数内核函数，用户态不能直接调用schedule）：
中断处理过程（包括时钟中断、IO中断、系统调用、异常）中,直接调用schedule；或者返回用户态时根据need_resched标记调用schedule
内核线程（是一个没有用户态只有内核态的特殊的进程，没有系统调用但可以发生中断，在发生中断的时候可以调用schedule）可以直接调用schedule进行进程切换。
用户态无法主动调度，只能通过陷入内核状态后的某个时机进行调度，即中断处理过程中

### 进程切换
为了控制进程的执行，内核必须有能力挂起正在CPU上执行的进程，并且恢复以前挂起的某个进程的执行
进程上下文包含了进程执行需要的所有信息：

1.用户地址空间（代码，数据，用户堆栈等）
2.控制信息（进程描述符，内核堆栈）
3.硬件上下文

Schedule函数选择新的进程来运行，调用context_switch进行切换，这个宏调用switch_to (prev指向当前进程，next指向被调度的进程)

<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/30.png" width = 93% height = 90% />  

下一篇继续记录（来自华中科技大学操作系统原理学习）