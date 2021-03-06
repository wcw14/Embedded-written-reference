## [Linux内核的五个子系统](https://blog.csdn.net/xiaomage_307_/article/details/79117881)

+ 进程调度模块、内存管理模块、文件系统模块、进程间通信模块、网络接口模块。

1. 进程调度模块（SCHED）
   
   + 作用：用来负责控制系统中的多个进程对CPU的访问；多个进程能在CPU中“微观串行，宏观并行”地执行；
     + 采用多级反馈队列的策略 + 时间片轮转RR + 进程上下文的切换；
   + 地位：进程调度处于系统的**中心位置**，内核中其他的子系统都**依赖**它，因为每个子系统都需要**挂起或恢复进程**
   
2. 内存管理模块（MM）

   + 作用：是控制多个进程安全地**共享**主内存区域
   + MMU：完成虚拟内存到物理内存的转换
   + 内存的管理：分页式管理，Linux下每页是4kb;

3. 虚拟文件系统(VFS) 

   ![VFS_kernel.png](/OS/photo/VFS_kernel.png)

   + 作用：
     + **隐藏**各种了硬件的具体细节，为所有的设备提供了统一的**接口**
     + **独立**于各个具体的文件系统，是对各种文件系统的一个**抽象**
   + 三部分：
     + 超级块super block：存放文件系统相关信息
     + 索引节点inode：存放文件的物理信息
     + 目录项dentry存放文件的逻辑信息
   
4. 网络接口

   + 作用：提供对多种网络通信标准的访问并支持许多网络硬件

5. 进程间通信

   + 作用：用于支持多种进程间的信息交换方式；
   + Linux支持进程间的多种通信机制，包含信号量、共享内存、管道等，这些机制可协助多个进程、多资源的互斥访问、进程间的同步和消息传递。

### 子系统之间的依赖关系

1. 进程调度和内存管理：
   + 相互**依赖**关系；
   + 程序要运行必须为之创建进程，而创建进程的第一件事情，就是将程序和数据装入内存。
2. 进程间通信和内存管理：
   + 进程间通信子系统要依赖内存管理支持共享**内存通信机制**；
   + 这种机制允许两个进程除了拥有自己的私有空间，还可以存取共同的内存区域。
3. 虚拟文件系统和网络接口
   + 虚拟文件系统：利用网络接口**支持**网络文件系统(NFS)，也利用内存管理支持RAMDISK设备。
4. 内存管理和虚拟文件系统
   + 内存管理利用虚拟文件系统支持交换，交换进程（swapd）定期由调度程序调度，这                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              也是内存管理依赖于进程调度的惟一原因
   + 当一个进程存取的内存映射被换出时，内存管理向文件系统发出请求，同时，挂起当前正在运行的进程。
5. 内核中的所有子系统还要依赖于一些共同的资源，如分配和释放内存空间的函数、打印警告或错误信息的函数及系统提供的调试例程等。