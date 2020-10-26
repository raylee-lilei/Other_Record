---
title: OS_FileManagement
author: raylee
avatar: 'https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.4/img/custom/logo_1.png'
authorLink: 
authorAbout: industry pays
authorDesc: industry pays
categories: technology
comments: true
date: 2020-04-02 20:20:33
tags:
keywords: FileManagement
description:
photos: https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.5/img/article/OS/os.jpg
---

# 设备管理

## 主要功能
1.设备分配
**共享设备分配**（包括所有的块设备，多进程交替使用同一设备。例如磁盘）
**独占设备分配**（包括所有的字符型设备，任意时间内最多被一个进程占用，例如：键盘，打印机）
使用过程：申请——》使用——》释放
**虚拟分配**在一类物理设备上模拟另一类物理设备，虚拟光驱

借助辅存部分区域模拟独占设备，将独占设备转化为共享设备；当进程需要与独占设备交换信息时，就采用虚拟技术将与该独占设备所对应的虚拟设备分配给他
例如SPOOLing系统(外部设备同时联机操作)
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/61.png" width = 53% height = 42% /> 
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/62.png" width = 53% height = 42% /> 

2.设备映射：逻辑设备映射到物理设备

3.设备驱动：对物理设备进行控制实现I/O操作；接受应用的服务请求；向用户提供统一接口

设备驱动程序的特点:
●它与硬件相关
●每一类的设备都有特定的驱动程序
●驱动程序一般由厂商根据操作系统要求编写
●操作系统仅对设备驱动的接口提出要求

## Linux内核模块
（解决单体内核机制的不足）
未经链接的可执行代码
经过装载成为内核的一部分
然后动态的装载和卸载
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/63.png" width = 33% height = 22% /> 

**编译模块：**
```bash
gcc -o xxmoudle.ko _D_KERNEL_ -MOUDLE xxx.c
```
**安装:**
```bash
sudo  insmod xxmoudle.ko    还可以带参数（Sudo  insmod xxmoudle.ko str = “xxx”  intval = 4）
```
**删除模块：**
```bash
sudo rmmod moudle
```
**查看内核信息:**
```bash
dmesg
```
**查看模块：**
```bash
Lsmod
```

## Linux驱动程序
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/64.png" width = 33% height = 22% /> 
驱动程序工作在内核态

1.驱动设备分为：
**字符设备**(执行ls -L/dev  c)
**块设备**（b）
**网络设备**（socket）	

硬件设备都可看成设备文件，用文件接口来完成设备的操作

2.一个完成linux的设备驱动程序的结构：
1）.设备打开
2）.释放
3）.读写操作
4）.控制操作
5）.中断和轮询处理
6）.驱动程序的注册和注销

3.简单字符设备驱动程序的例子：

**定义5个接口**
```bash
static int my_open(struct inode * inode,struct file * filp){
//设备打开的操作
MOU_INC_USE_COUNT; //增加引用计数
return 0;
}

static int my_release(struct inode* inode,struct file * filp){
//设备关闭时的操作
MOU_DEC_USE_COUNT;
return 0;
}

static int my_write(struct file *file,const char* buffer,size loff_t *ppos){
//设备写入时的操作
char led_status = 0;
copy_from_user(&let_status,buffer,sizeof(led_status)); //从用户态到内核态
if(led_tatus ==0x01){
AT91F_PIOB_SetOutput(LED)
}else{
AT91F_PIOB_ClearOutput(LED)
}
return 0;
}

static int my_init(void){
//设备的注册：硬件初始化，注册设备，创建设备节点
//硬件初始化
AT91F_PIOB_Enable(LED);
//字符设备注册
Led_Major = register_chrdev(0,DEVICE_NAME,&my_fops);
//创建设备文件
Devfs_Led_Dir = devfs_mk_dir(NULL,”my_led”,NULL);
Devds_Led_Raw = devfs_register(Devfs_Led_Dir,”0”,DEVFS_FL_DEFAULT,Led_Major,1,S_IFCHR|S_IRUSR|S_IWUSR,&my_fops,NULL);
}

static void my_exit(void){
//设备的注销：删除设备节点，注销设备...
//删除设备文件
devfs_unregister(Devfs_Led_Raw);
devfs_unregister(Devfs_Led_Dir);
//注销设备
unregister_chrdev(Led_Major,DEVICE_NAME);

}
```

```bash
moudle_init(my_init);设备注册的登记
moudle_exit(my_exit);//设备注销的登记
```

文件操作有国际意义的结构体定义，里面的参数不是每一个都会用到
以上使我们自定义的函数，需要把我们自定义的函数和标准的文件架构关联起来
```bash
static struct file_operations my_fops{
open: my_open;
write:my_write;
release:my_release;
}
```

最终make之后生成my_led.o的驱动程序

驱动程序插入内核：
```bash
insmod my_led.o
```
查看是否载入:
```bash
cat /proc/devices
```
移除：
```bash
rmmod my_led
```

测试驱动应用程序
```bash
int main(void){
int fd;
char led_on = 1;
fd  = open(“/dev/my_led”,O_RDWR);
write(fd,&led_on,1);
close(fd);
return 0;
}
```

# 文件系统
## 连续文件
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/65.png" width = 43% height = 32% /> 
## 索引文件
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/66.png" width = 43% height = 32% /> 
## 串联文件
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/67.png" width = 60% height = 49% /> 
例如FAT文件（FAT12   FAT16  FAT32 ）
将next 集中顺序放在FAT表中
若存储块有2^N块， FAT有2^N个元素，每项至少需要N位宽度
扇区：磁盘上最小寻址存储单元  512 字节
簇：存储块，设备的最小	存取单元
FAT元素的数目和簇测的数目一样
磁盘容量= FAT长度*簇容量=FAT长度*簇扇区数*512

## 存储空间管理
### 空闲文件目录
把连续空闲看成特殊文件，由多个连续空闲块组成
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/68.png" width = 53% height = 42% /> 
### 空闲块链
把空闲块链接在一起 ，当申请空闲块时，链头开始搜索所需空闲块，当回首空闲块时，把释放的空闲块加载链位
### 位示图
<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/69.png" width = 48% height = 37% /> 

<img src="https://cdn.jsdelivr.net/gh/raylee-lilei/cdn@2.6/img/article/OS/70.png" width = 46% height = 35% /> 
