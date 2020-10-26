---
title: OS_MemoryManagement
author: raylee
avatar: 'https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/custom/logo_1.png'
authorLink: 
authorAbout: industry pays
authorDesc: industry pays
categories: technology
comments: true
date: 2020-04-02 20:21:30
tags: 
keywords: MemoryManagement
description:
photos: https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/os.jpg
---

# 存储管理

## 存储器功能
容量大、速度快、信息永久保存、多道程序并行（共享，保护） 

## 内存管理功能
### 地址映射
把程序的地址（虚拟地址/逻辑地址/虚地址），变换为内存中真实的地址（实地址/物理地址）

地址映射有三种方式：

1.固定地址映射（编程时直接使用物理地址或者通过编译器映射物理地址 ）
2.静态地址映射（程序装入时由操作系统完成逻辑到物理的过程，装入时是指：双击图标时/终端输入命令回车时）
**物理地址（MA）=逻辑地址（VA）+装入基址（BA）**
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/31.png" width = 53% height = 40% />  
                                        
MA = X + BA
●程序运行之前确定映射关系 	
●程序装入后不能移动
●程序占用连续空间

3.动态地址映射
程序在执行过程中将逻辑转化为物理地址
内存占用空间可动态变化
程序不用占用连续的内存空间
便于多个进程共享代码

### 虚拟存储

#### 内存分配
分配空间
#### 存储保护
防止越界或者越权。采用界址寄存器，在CPU中设置一对上限寄存器和下限寄存器存储程序在内存中的上限和下限

## 物理内存管理
**单一区存储**
**分区存储**
1.固定分区（找合适的分区运行）
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/32.png" width = 53% height = 40% />  

2.动态分区（存在空间碎片）
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/33.png" width = 43% height = 30% />  
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/34.png" width = 43% height = 30% />  


**空闲区放置策略**
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/35.png" width = 38% height = 25% />  
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/36.png" width = 43% height = 30% />  
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/37.png" width = 43% height = 30% />  

1.分区回收（释放与现有空闲区相邻，先合并再更新空闲区，不相邻直接插入空闲区）
2.内存覆盖技术
	用较小的空间运行较大的程序
有常驻区和覆盖区
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/38.png" width = 48% height = 35% /> 
编程复杂
程序执行时间变长：从外存装入内存耗时

3.内存交换技术
内存不够时把进程写入磁盘，运行时再换回内存
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/39.png" width = 48% height = 35% /> 
要运行B 先换出A，运行B。当要运行A的时候再把A换回内存。此时A放入内存的什么位置？
（1）.放到原来的位置：简单，可能会造成地址冲突
（2）.放到任意位置，灵活运用内存，地址需要重定位

**解决碎片问题**
1.规定门限值，分割空闲区域时，若剩余部分小于门限值，不分割，全部分配给用户
2.内存拼接技术：将所有空闲区集中构成一个大的空闲区（消耗资源系统，离线拼接，重新定义作业）
(1)在释放回收的时候：频率大，内存开销大
(2)系统找不到空间的时候：空闲区管理
(3)定期：空闲区管理
3.解除程序占用连续内存才能运行的限制
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/40.png" width = 45% height = 32% /> 


## 虚拟内存
●物理内存有以下几种缺点：
源程序知直接使用内存的物理地址，程序访问容易引起冲突
程序必须全部装入内存才能运行，内存小而不能满足
程序占有连续的一片内存，产生内存碎片
多程序同事运行相互干扰

### 虚拟内存
使得较大的和多个程序在小的内存中运行
多个程序并发运行地址不冲突
内存利用率高，无碎片

### 实现思路
程序运行时，只把当前必要的很小一部分代码和数据装入内存，其余的需要的时候再装入，不运行的代码和数据及时从内存中删除

### 典型的虚拟内存管理方式
#### 页式虚拟内存管理
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/41.png" width = 43% height = 30% /> 

只把程序部分页装入内存中便可运行
页在内存中占用页框不比相邻
需要页时按需从硬盘中调入内存
不运行的页，及时删除

**1.页式地址映射**
虚拟地址（VA）可以分解成页号（P）和页内偏移（W）
若页的大小（2^n）
P = VA / 页的大小  = VA >> n 
W= VA %页的大小  =低n位 = VA&&（2^n -1）
由于页是直接平移到页框上，他们直接的对应关系使用页表记录
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/42.png" width = 36% height = 26% /> 
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/43.png" width = 36% height = 26% /> 

**虚拟地址（页式）——> 物理地址**
1).从VA中分离P和W
2).查页表，搜索某个页号P在页框中的索引P1
3).某个页号物理地址MA = P1 * X页的大小 +W

**2.块表机制（cache）**
慢表：页表放在内存中
快表：页表放在cache中
快表容量小，访问快，他是慢表的部分内容复制；地址映射优先访问快快表，若快表中找到所虚的数据，叫命中，若果没有命中需要访问慢表，然后在更新快表。
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/44.png" width = 43% height = 33% /> 

普通页表采用软件对比，快表采用硬件对比

**3.页面的共享**
可以节省空间。比如Windows系统的动态链接库是程序共享的
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/45.png" width = 53% height = 42% /> 

不同的进程有相同的页框号，多个进程访问相同的内存
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/46.png" width = 38% height = 27% /> 

在页表中加入中断位，用来标识页是否在内存中，不在内存用1 ；在，用0标识。
在页表中加入访问位和修改位，0未修改，未访问；1 修改过，访问过
缺页中断，在地址映射过程中，所访问的目的也不在内存中，则系统产生异常中断（缺页中断）
缺页中断程序，把所缺的页从辅存调入内存中的某个页框，并更某个页框号，修改中断位为0
访存指令：
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/47.png" width = 58% height = 46% /> 

缺页率 = 缺页次数/访问页面总次数

**4.页面淘汰算法**
页面在内存和辅存之间频繁交换叫做抖动
1).最佳算法（OPT）
淘汰以后不再需要或者最远的将来才会用到的页面
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/48.png" width = 43% height = 32% /> 
理想状态，无法实现

2).先进先出淘汰算法（FIFO）
淘汰在内存中停留时间最长的页面
实现简单，但是进程只有按顺序访问地址空间时，页面命中率才很理想，随着页框数增加，缺页率反而增加

3).淘汰最长时间未被使用的页面（LRU）
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/49.png" width = 43% height = 32% /> 
通过移位寄存器来实现，每个页面被访问则将其重置1，周期性的左移，记录值。

4).最不经常使用算法（LFU）
访问次数最少的淘汰

**5.缺页的因素**
●淘汰算法
●分配的页框数（页框越少，越容易缺页）
●页本身的大小（页面越小，越容易缺页）
●程序的编制方法
●由于局部性原理，上面的程序运行效率更好，二维数组按行存储，下面按列遍历会产生跳转，会导致更高的缺页率，局部性差
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/50.png" width = 33% height = 22% /> 


**6.页式系统的不足**
页面划分无逻辑
页的共享不灵活
页内碎片

#### 段式存储管理
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/51.png" width = 43% height = 32% /> 
把进程按逻辑意义划分多个段，一段为单位，每个段连续存储，每个段间是独立的，段与段间不相邻
段式虚拟地址（VA）有段号，段内偏移（W）

段表 记录段号（S），段长（L），该段的在内存的基地址即基地址（B）

MA = B+W 
0≤W≤L
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/52.png" width = 50% height = 39% /> 

**1.段氏缺点**
段需要连续存储
段的最大尺寸收到内存大小的限制
在辅存中管理可变尺寸的段比较困难

**2.段与页的区别**
段长可变 ，页面大小固定
段的划分有意义，页的划分无意义
段方便共享，页面共享不方便
段是程序员划分的，页面用户不可见
段偏移有溢出，页面无偏移溢出

#### 段页式存储管理
段中划分页面
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/53.png" width = 33% height = 22% /> 

段页式地址映射过程
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/54.png" width = 53% height = 42% /> 


## Intel CPU的物理结构

### 实模式 
20 位，1M内存空间
地址表示方式：段地址（16位）+偏移地址（16位） 
直接存储物理地址
### 保护模式
32位，4G内存
多增加新的寄存器

●开机从实模式转到保护模式

**控制寄存器CR0：**
PE位：0实模式；1 保护模式
PG允许分页

**控制器CR2：**
保存缺页的线性地址
控制器CR3：
页目录的基址
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/55.png" width = 48% height = 37% /> 

逻辑地址在实模式下是段基址+偏移值
逻辑地址在保护模式下段选择子+偏移值

### 段与段描述符
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/56.png" width = 42% height = 30% /> 

**描述符数据结构**
（段基址：32位    段界限： 20 位）
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/57.png" width = 48% height = 38% /> 
描述符表里面放的描述符

**描述符表的类型**
全局描述符（GDT）：包含所有进程	的段描述符
局部描述符（LDT）：特定进程有关的描述符
中断描述符（IDT）：包含中断服务程序段描述符

### 选择子
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/58.png" width = 43% height = 32% /> 
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/59.png" width = 43% height = 32% /> 

## Linux三级页表结构
将4M的超大页表存储到1K个页框中。页表按需调入内存
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/60.png" width = 53% height = 42% /> 

### Linux段机制
用户空间：3G   0—0xBFFFFFFF
内核空间：1G   0xC0000000—0xFFFFFFFF
进程创建时，段机制对寄存器初始化

●内核特权级为0
●用户特权级为3 

作用
●利用段机制隔离用户数据和系统数据
●简化逻辑到线性地址的转化，可直接将虚拟地址当做线性地址

下一篇继续记录（来自华中科技大学操作系统原理学习）