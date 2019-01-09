#### 操作系统解析
操作系统(operating system)是管理计算机硬件与软件资源的计算机程序，同时也是计算机系统的内核与基石。操作系统需要处理如管理与配置内存、决定系统资源供需的优先次序、控制输入与输出设备、操作网络与管理文件系统等基本事务。操作系统也提供一个让用户与系统交互的操作界面。

操作系统的类型非常多样，不同机器安装的操作系统可从简单到复杂，可从移动电话的嵌入式系统到超级计算机的大型操作系统。许多操作系统制造者对它涵盖范畴的定义也不尽一致，例如有些操作系统集成了图形用户界面，而有些仅使用命令行界面，而将图形用户界面视为一种非必要的应用程序。

接着我们对操作系统详细解析下。

* [操作系统基本特征](#操作系统基本特征)
  * [操作系统基本功能](#操作系统基本功能)
  * [系统调用](#系统调用)
  * [大内核和微内核](#大内核和微内核)
  * [中断分类](#中断分类)
* [进程管理](#进程管理)
  * [进程与线程](#进程与线程)
  * [进程状态的切换](#进程状态的切换)
  * [进程调度算法](#进程调度算法)
  * [进程同步](#进程同步)
  * [进程通信](#进程通信)
  * [线程间通信和进程间通信](#线程间通信和进程间通信)
  * [进程操作](#进程操作)
  
#### 操作系统基本特征

1. 并发和并行

并发是指宏观上在一段时间内能同时运行多个程序，而并行则指同一时刻能运行多个指令。

并行需要硬件支持，如多流水线或者多处理器。

操作系统通过引入进程和线程，使得程序能够并发运行。
<p align="center">
<img width="500" align="center" src="../images/20.jpg" />
</p>

2. 共享

共享是指系统中的资源可以被多个并发进程共同使用。

有两种共享方式：互斥共享和同时共享。

互斥共享的资源称为临界资源，例如打印机等，在同一时间只允许一个进程访问，需要用同步机制来实现对临界资源的访问。

3. 虚拟

虚拟技术把一个物理实体转换为多个逻辑实体。

利用多道程序设计技术，让每个用户都觉得有一个计算机专门为他服务。

主要有两种虚拟技术：时分复用技术和空分复用技术。例如多个进程能在同一个处理器上并发执行使用了时分复用技术，让每个进程轮流占有处理器，每次只执行一小个时间片并快速切换。

4.异步

异步指进程不是一次性执行完毕，而是走走停停，以不可知的速度向前推进。

但只要运行环境相同，OS需要保证程序运行的结果也要相同。

#### 操作系统基本功能

1. 进程管理

进程控制、进程同步、进程通信、死锁处理、处理机调度等。

2. 内存管理

内存分配、地址映射、内存保护与共享、虚拟内存等。

3. 文件管理

文件存储空间的管理、目录管理、文件读写管理和保护等。

4. 设备管理

完成用户的 I/O 请求，方便用户使用各种设备，并提高设备的利用率。

主要包括缓冲管理、设备分配、设备处理、虛拟设备等。

#### 系统调用

如果一个进程在用户态需要使用内核态的功能，就进行系统调用从而陷入内核，由操作系统代为完成。
<p align="center">
<img width="600" align="center" src="../images/21.jpg" />
</p>

Linux 的系统调用主要有以下这些：

| Task     | Commands                    |
| -------- | --------------------------- |
| 进程控制 | fork(); exit(); wait();     |
| 进程通信 | pipe(); shmget(); mmap();   |
| 文件操作 | open(); read(); write();    |
| 设备操作 | ioctl(); read(); write();   |
| 信息维护 | getpid(); alarm(); sleep(); |
| 安全     | chmod(); umask(); chown();  |

#### 大内核和微内核

* 大内核

大内核是将操作系统功能作为一个紧密结合的整体放到内核。

由于各模块共享信息，因此有很高的性能。

* 微内核

由于操作系统不断复杂，因此将一部分操作系统功能移出内核，从而降低内核的复杂性。移出的部分根据分层的原则划分成若干服务，相互独立。

在微内核结构下，操作系统被划分成小的、定义良好的模块，只有微内核这一个模块运行在内核态，其余模块运行在用户态。

因为需要频繁地在用户态和核心态之间进行切换，所以会有一定的性能损失。

<p align="center">
<img width="600" align="center" src="../images/22.jpg" />
</p>

#### 中断分类

1. 外中断

由 CPU 执行指令以外的事件引起，如 I/O 完成中断，表示设备输入/输出处理已经完成，处理器能够发送下一个输入/输出请求。此外还有时钟中断、控制台中断等。

2. 异常

由 CPU 执行指令的内部事件引起，如非法操作码、地址越界、算术溢出等。

3. 陷入

在用户程序中使用系统调用。

| 类型     | 源头                     | 响应方式   | 处理机制                             |
| -------- | ------------------------ | ---------- | ------------------------------------ |
| 中断     | 外设                     | 异步       | 持续，对用户应用程序是透明的         |
| 异常     | 应用程序意想不到的行为   | 同步       | 杀死或重新执行意想不到的应用程序指令 |
| 系统调用 | 应用程序请求操作提供服务 | 异步或同步 | 等待和持续                           |

6. 什么是堆和栈？说一下堆栈都存储哪些数据？

栈区（stack）— 由**编译器**自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

堆区（heap） — 一般由**程序员分配释放**， 若程序员不释放，程序结束时可能由OS回收 。

数据结构中这两个完全就不放一块来讲，数据结构中栈和队列才是好基友，我想新手也很容易区分。 

我想需要区分的情况肯定不是在数据结构话题下，而大多是在 OS 关于不同对象的内存分配这块上。 

简单讲的话，在 C 语言中： 

```c
int a[N];   // go on a stack
int* a = (int *)malloc(sizeof(int) * N);  // go on a heap
```

<p align="center">
<img width="600" align="center" src="../images/23.jpg" />
</p>

7. 分布式锁

分布式锁，是控制分布式系统之间同步访问共享资源的一种方式。在分布式系统中，常常需要协调他们的动作。如果不同的系统或是同一个系统的不同主机之间共享了一个或一组资源，那么访问这些资源的时候，往往需要互斥来防止彼此干扰来保证一致性，在这种情况下，便需要使用到分布式锁。


#### 进程管理

#### 进程与线程
<p align="center">
<img width="600" align="center" src="../images/24.jpg" />
</p>

1. 进程

**进程是资源分配的基本单位**，用来管理资源（例如：内存，文件，网络等资源）

进程控制块 (Process Control Block, PCB) 描述进程的基本信息和运行状态，所谓的创建进程和撤销进程，都是指对 PCB 的操作。**（PCB是描述进程的数据结构）**
<p align="center">
<img width="600" align="center" src="../images/25.jpg" />
</p>

上面是4 个程序创建了 4 个进程，这 4 个进程可以并发地执行。

2. 线程

线程是独立调度的基本单位。

一个进程中可以有多个线程，它们共享进程资源。

QQ 和浏览器是两个进程，浏览器进程里面有很多线程，例如 HTTP 请求线程、事件响应线程、渲染线程等等，线程的并发执行使得在浏览器中点击一个新链接从而发起 HTTP 请求时，浏览器还可以响应用户的其它事件。

3. 区别

（一）拥有资源

进程是资源分配的基本单位，但是线程不拥有资源，线程可以访问隶属进程的资源。

（二）调度

线程是独立调度的基本单位，在同一进程中，线程的切换不会引起进程切换，从一个进程内的线程切换到另一个进程中的线程时，会引起进程切换。

（三）系统开销

由于创建或撤销进程时，系统都要为之分配或回收资源，如内存空间、I/O 设备等，所付出的开销远大于创建或撤销线程时的开销。类似地，在进行进程切换时，涉及当前执行进程 CPU 环境的保存及新调度进程 CPU 环境的设置，而线程切换时只需保存和设置少量寄存器内容，开销很小。

（四）通信方面

进程间通信 (IPC) 需要进程同步和互斥手段的辅助，以保证数据的一致性。而线程间可以通过直接读/写同一进程中的数据段（如全局变量）来进行通信。

#### 进程状态的切换
<p align="center">
<img width="600" align="center" src="../images/26.jpg" />
</p>

就绪状态（ready）：等待被调度
运行状态（running）
阻塞状态（waiting）：等待资源

需要注意的是:
* 只有就绪态和运行态可以相互转换，其它的都是单向转换。就绪状态的进程通过调度算法从而获得 CPU 时间，转为运行状态；而运行状态的进程，在分配给它的 CPU 时间片用完之后就会转为就绪状态，等待下一次调度。
* 阻塞状态是缺少需要的资源从而由运行状态转换而来，但是该资源不包括 CPU 时间，缺少 CPU 时间会从运行态转换为就绪态。
* 进程只能自己阻塞自己，因为只有进程自身才知道何时需要等待某种事件的发生

#### 进程调度算法
相信很多人都听说过进程调度的算法，但是不同环境的调度算法目标不同，因此需要针对不同环境来讨论调度算法。

1. 批处理系统

批处理系统没有太多的用户操作，在该系统中，调度算法目标是保证吞吐量和周转时间（从提交到终止的时间）。

*  先来先服务
先来先服务 first-come first-serverd（FCFS）,按照请求的顺序进行调度。这样可以有利于长作业，但不利于短作业，因为短作业必须一直等待前面的长作业执行完毕才能执行，而长作业又需要执行很长时间，造成了短作业等待时间过长。

* 短作业优先

短作业优先 shortest job first（SJF）,按估计运行时间最短的顺序进行调度。长作业有可能会饿死，处于一直等待短作业执行完毕的状态。因为如果一直有短作业到来，那么长作业永远得不到调度。

* 最短剩余时间优先

最短剩余时间优先 shortest remaining time next（SRTN）,按估计剩余时间最短的顺序进行调度。

2. 交互式系统

交互式系统有大量的用户交互操作，在该系统中调度算法的目标是快速地进行响应。

* 时间片轮转算法
将所有进程按FCFS （先来先服务） 的原则排成一个队列，每次调度时，把 CPU 时间分配给队首进程，该进程可以执行一个时间片。当时间片用完时，由计时器发出时钟中断，调度程序便停止该进程的执行，并将它送往就绪队列的末尾，同时继续把 CPU 时间分配给队首的进程。

时间片轮转算法的效率和时间片的大小有很大关系。因为进程切换都要保存进程的信息并且载入新进程的信息，如果时间片太小，会导致进程切换得太频繁，在进程切换上就会花过多时间。
<p align="center">
<img width="500" align="center" src="../images/27.jpg" />
</p>

* 优先级调度

为每个进程分配一个优先级，按优先级进行调度。

为了防止低优先级的进程永远等不到调度，可以随着时间的推移增加等待进程的优先级。

* 多级反馈队列

通常，如果一个进程需要执行 100 个时间片，如果采用时间片轮转调度算法，那么需要交换 100 次。

多级队列是为这种需要连续执行多个时间片的进程考虑，它设置了多个队列，每个队列时间片大小都不同，例如 1,2,4,8,..。进程在第一个队列没执行完，就会被移到下一个队列。这种方式下，之前的进程只需要交换 7 次。

每个队列优先权也不同，最上面的优先权最高。因此只有上一个队列没有进程在排队，才能调度当前队列上的进程。

可以将这种调度算法看成是时间片轮转调度算法和优先级调度算法的结合。

<p align="center">
<img width="500" align="center" src="../images/28.jpg" />
</p>

3. 实时系统

实时系统要求一个请求在一个确定时间内得到响应。

分为硬实时和软实时，前者必须满足绝对的截止时间，后者可以容忍一定的超时。

#### 进程同步

* 临界区

对临界资源进行访问的那段代码称为临界区。

为了互斥访问临界资源，每个进程在进入临界区之前，需要先进行检查。
```markdown
 entry section
 critical section;
 exit section
```
* 同步与互斥

同步：多个进程按一定顺序执行；

互斥：多个进程在同一时刻只有一个进程能进入临界区。

* 信号量
```markdown
P 和 V 是来源于两个荷兰语词汇，P() ---prolaag （荷兰语，尝试减少的意思），V() ---verhoog（荷兰语，增加的意思）
```
信号量（Semaphore）是一个整型变量，可以对其执行 down 和 up 操作，也就是常见的 P 和 V 操作。

* down : 如果信号量大于 0 ，执行 -1 操作；如果信号量等于 0，进程睡眠，等待信号量大于 0；（阻塞）
* up ：对信号量执行 +1 操作，唤醒睡眠的进程让其完成 down 操作。（唤醒）

down 和 up 操作需要被设计成原语，不可分割，通常的做法是在执行这些操作的时候屏蔽中断。

如果信号量的取值只能为 0 或者 1，那么就成为了 互斥量（Mutex） ，0 表示临界区已经加锁，1 表示临界区解锁。

```c
typedef int semaphore;
semaphore mutex = 1;
void P1() {
    down(&mutex);
    // 临界区
    up(&mutex);
}

void P2() {
    down(&mutex);
    // 临界区
    up(&mutex);
}
```
* 使用信号量实现生产者-消费者问题

通常，使用一个缓冲区来保存数据，只有缓冲区没有满，生产者才可以放入数据；只有缓冲区不为空，消费者才可以拿走数据。

因为缓冲区属于临界资源，因此需要使用一个互斥量 mutex 来控制对缓冲区的互斥访问。

为了同步生产者和消费者的行为，需要记录缓冲区中数据的数量。数量可以使用信号量来进行统计，这里需要使用两个信号量：empty 记录空缓冲区的数量，full 记录满缓冲区的数量。其中，empty 信号量是在生产者进程中使用，当 empty 不为 0 时，生产者才可以放入数据；full 信号量是在消费者进程中使用，当 full 信号量不为 0 时，消费者才可以取走数据。

这里需要注意，不能先对缓冲区进行加锁，再测试信号量。也就是说，不能先执行 down(mutex) 再执行 down(empty)。如果这么做了，那么可能会出现这种情况：生产者对缓冲区加锁后，执行 down(empty) 操作，发现 empty = 0，此时生产者睡眠。消费者不能进入临界区，因为生产者对缓冲区加锁了，也就无法执行 up(empty) 操作，empty 永远都为 0，那么生产者和消费者就会一直等待下去，造成死锁。

```c
#define N 100
typedef int semaphore;
semaphore mutex = 1;
semaphore empty = N;
semaphore full = 0;

void producer() {
    while(TRUE){
        int item = produce_item(); // 生产一个产品
        // down(&empty) 和 down(&mutex) 不能交换位置，否则造成死锁
        down(&empty); // 记录空缓冲区的数量，这里减少一个产品空间
        down(&mutex); // 互斥锁
        insert_item(item);
        up(&mutex); // 互斥锁
        up(&full); // 记录满缓冲区的数量，这里增加一个产品
    }
}

void consumer() {
    while(TRUE){
        down(&full); // 记录满缓冲区的数量，减少一个产品
        down(&mutex); // 互斥锁
        int item = remove_item();
        up(&mutex); // 互斥锁
        up(&empty); // 记录空缓冲区的数量，这里增加一个产品空间
        consume_item(item);
    }
}
```
* 管程

管程 (Monitors，也称为监视器) 是一种程序结构，结构内的多个子程序（对象或模块）形成的多个工作线程互斥访问共享资源。

使用信号量机制实现的生产者消费者问题需要客户端代码做很多控制，而管程把控制的代码独立出来，不仅不容易出错，也使得客户端代码调用更容易。

管程是为了解决信号量在临界区的 PV 操作上的配对的麻烦，把配对的 PV 操作集中在一起，生成的一种并发编程方法。其中使用了条件变量这种同步机制。

c 语言不支持管程，下面的示例代码使用了类 Pascal 语言来描述管程。示例代码的管程提供了 insert() 和 remove() 方法，客户端代码通过调用这两个方法来解决生产者-消费者问题。

```c
monitor ProducerConsumer
    integer i;
    condition c;

    procedure insert();
    begin
        // ...
    end;

    procedure remove();
    begin
        // ...
    end;
end monitor;
```
管程有一个重要特性：在一个时刻只能有一个进程使用管程。进程在无法继续执行的时候不能一直占用管程，否者其它进程永远不能使用管程。

管程引入了 条件变量 以及相关的操作：wait() 和 signal() 来实现同步操作。对条件变量执行 wait() 操作会导致调用进程阻塞，把管程让出来给另一个进程持有。signal() 操作用于唤醒被阻塞的进程。

* 经典同步问题

1. 读-写问题

允许多个进程同时对数据进行读操作，但是不允许读和写以及写和写操作同时发生。

Rcount：读操作的进程数量（Rcount=0）

CountMutex：对于Rcount进行加锁（CountMutex=1）

WriteMutex：互斥量对于写操作的加锁（WriteMutex=1）
```c
Rcount = 0;
semaphore CountMutex = 1;
semaphore WriteMutex = 1;

void writer(){
    while(true){
        sem_wait(WriteMutex);
        // TO DO write();
        sem_post(WriteMutex);
    }
}

// 读者优先策略
void reader(){
    while(true){
        sem_wait(CountMutex);
        if(Rcount == 0)
            sem_wait(WriteMutex);
        Rcount++;
        sem_post(CountMutex);
        
        // TO DO read();
        
        sem_wait(CountMutex);
        Rcount--;
        if(Rcount == 0)
            sem_post(WriteMutex);
        sem_post(CountMutex);
	}
}
```
2. 哲学家进餐问题

<p align="center">
<img width="500" align="center" src="../images/29.jpg" />
</p>
五个哲学家围着一张圆桌，每个哲学家面前放着食物。哲学家的生活有两种交替活动：吃饭以及思考。当一个哲学家吃饭时，需要先拿起自己左右两边的两根筷子，并且一次只能拿起一根筷子。

第一种方案:

下面是一种错误的解法，考虑到如果所有哲学家同时拿起左手边的筷子，那么就无法拿起右手边的筷子，造成死锁。
```c
#define N 5		   // 哲学家个数
void philosopher(int i)    // 哲学家编号：0 － 4
{
    while(TRUE)
    {
        think();			// 哲学家在思考
        take_fork(i);			// 去拿左边的叉子
        take_fork((i + 1) % N);	// 去拿右边的叉子
        eat();				// 吃面条中….
        put_fork(i);			// 放下左边的叉子
        put_fork((i + 1) % N);	// 放下右边的叉子
    }
}
```

第二种方案:

对拿叉子的过程进行了改进，但仍不正确.
```c
#define N 5	 // 哲学家个数
while(1)  // 去拿两把叉子
{       
    take_fork(i);			// 去拿左边的叉子
    if(fork((i+1)%N)) {		// 右边叉子还在吗
    	take_fork((i + 1) % N);// 去拿右边的叉子
    	break;			// 两把叉子均到手
    }
    else {				// 右边叉子已不在
    	put_fork(i);		// 放下左边的叉子
    	wait_some_time();	// 等待一会儿
    }
}
```
第三种方案:

等待时间随机变化。可行，但非万全之策.
```c
#define N 5	 // 哲学家个数
while(1)  // 去拿两把叉子
{       
	take_fork(i);			// 去拿左边的叉子
	if(fork((i+1)%N)) {		// 右边叉子还在吗
	    take_fork((i + 1) % N);// 去拿右边的叉子
	    break;			// 两把叉子均到手
	}
	else {				// 右边叉子已不在
	    put_fork(i);		// 放下左边的叉子
	    wait_random_time( );	// 等待随机长时间
	}
}
```
第四种方案:

互斥访问。正确，但每次只允许一人进餐
```c
semaphore mutex	   // 互斥信号量，初值1
void philosopher(int i)  // 哲学家编号i：0－4	
{
	while(TRUE){
	    think();			// 哲学家在思考
	    P(mutex);			// 进入临界区
	    take_fork(i);			// 去拿左边的叉子
	    take_fork((i + 1) % N);	// 去拿右边的叉子
	    eat();				// 吃面条中….
	    put_fork(i);			// 放下左边的叉子
	    put_fork((i + 1) % N);	// 放下右边的叉子
	    V(mutex);			// 退出临界区
	}
}
```
正确方案如下：

为了防止死锁的发生，可以设置两个条件（临界资源）：

* 必须同时拿起左右两根筷子；
* 只有在两个邻居都没有进餐的情况下才允许进餐。

```c
//1. 必须由一个数据结构，来描述每个哲学家当前的状态
#define N 5
#define LEFT i // 左邻居
#define RIGHT (i + 1) % N    // 右邻居
#define THINKING 0
#define HUNGRY   1
#define EATING   2
typedef int semaphore;
int state[N];                // 跟踪每个哲学家的状态

//2. 该状态是一个临界资源，对它的访问应该互斥地进行
semaphore mutex = 1;         // 临界区的互斥

//3. 一个哲学家吃饱后，可能要唤醒邻居，存在着同步关系
semaphore s[N];              // 每个哲学家一个信号量

void philosopher(int i) {
    while(TRUE) {
        think();
        take_two(i);
        eat();
        put_tow(i);
    }
}

void take_two(int i) {
    P(&mutex);  // 进入临界区
    state[i] = HUNGRY; // 我饿了
    test(i); // 试图拿两把叉子
    V(&mutex); // 退出临界区
    P(&s[i]); // 没有叉子便阻塞
}

void put_tow(i) {
    P(&mutex);
    state[i] = THINKING;
    test(LEFT);
    test(RIGHT);
    V(&mutex);
}

void test(i) {         // 尝试拿起两把筷子
    if(state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] !=EATING) {
        state[i] = EATING;
        V(&s[i]); // 通知第i个人可以吃饭了
    }
}
```

#### 进程通信
进程同步与进程通信很容易混淆，它们的区别在于：

* 进程同步：控制多个进程按一定顺序执行
* 进程通信：进程间传输信息

进程通信是一种手段，而进程同步是一种目的。也可以说，为了能够达到进程同步的目的，需要让进程进行通信，传输一些进程同步所需要的信息。

进程通信方式：

<p align="center">
<img width="500" align="center" src="../images/30.jpg" />
</p>

* 直接通信
发送进程直接把消息发送给接收进程，并将它挂在接收进程的消息缓冲队列上，接收进程从消息缓冲队列中取得消息。

Send 和 Receive 原语的使用格式如下：
```c
Send(Receiver,message);//发送一个消息message给接收进程Receiver
Receive(Sender,message);//接收Sender进程发送的消息message
```
* 间接通信

间接通信方式是指进程之间的通信需要通过作为共享数据结构的实体。该实体用来暂存发送进程发给目标进程的消息。

发送进程把消息发送到某个中间实体中，接收进程从中间实体中取得消息。这种中间实体一般称为信箱，这种通信方式又称为信箱通信方式。该通信方式广泛应用于计算机网络中，相应的通信系统称为电子邮件系统。

* 管道

管道是通过调用 pipe 函数创建的，fd[0] 用于读，fd[1] 用于写。
```c
#include <unistd.h>
int pipe(int fd[2]);
```
它具有以下限制：
* 只支持半双工通信（单向传输）
* 只能在父子进程中使用

<p align="center">
<img width="500" align="center" src="../images/31.jpg" />
</p>

* 命名管道

命名管道的作用是，管道只能在父子进程中使用的限制。
```c
#include <sys/stat.h>
int mkfifo(const char *path, mode_t mode);
int mkfifoat(int fd, const char *path, mode_t mode);
```
FIFO 常用于客户-服务器应用程序中，FIFO 用作汇聚点，在客户进程和服务器进程之间传递数据。

<p align="center">
<img width="500" align="center" src="../images/32.jpg" />
</p>

* 消息队列

消息队列相比于 FIFO，消息队列具有以下优点：

消息队列可以独立于读写进程存在，从而避免了 FIFO 中同步管道的打开和关闭时可能产生的困难；
* 避免了 FIFO 的同步阻塞问题，不需要进程自己提供同步方法；
* 读进程可以根据消息类型有选择地接收消息，而不像 FIFO 那样只能默认地接收。

* 信号量

它是一个计数器，用于为多个进程提供对共享数据对象的访问。

* 共享内存

允许多个进程共享一个给定的存储区。因为数据不需要在进程之间复制，所以这是最快的一种 IPC。

需要使用信号量用来同步对共享存储的访问。

多个进程可以将同一个文件映射到它们的地址空间从而实现共享内存。另外 XSI 共享内存不是使用文件，而是使用使用内存的匿名段。

* 套接字

套接字与其它通信机制不同的是，它可用于不同机器间的进程通信。


#### 线程间通信和进程间通信

* 线程间通信

1. synchronized同步

synchronized同步本质上就是 “共享内存” 式的通信。多个线程需要访问同一个共享变量，谁拿到了锁（获得了访问权限），谁就可以执行。

2. while轮询的方式

while轮询的方式就是，ThreadA 不断地改变条件，ThreadB 不停地通过 while 语句检测这个条件 (list.size()==5) 是否成立 ，从而实现了线程间的通信。但是这种方式会浪费 CPU 资源。这个有点浪费资源。

3. wait/notify机制

当条件未满足时，ThreadA 调用 wait() 放弃 CPU，并进入阻塞状态。（不像 while 轮询那样占用 CPU）

当条件满足时，ThreadB 调用 notify() 通知线程 A，所谓通知线程 A，就是唤醒线程 A，并让它进入可运行状态。

 
 * 进程间通信
 
1. 管道（Pipe）:管道可用于具有亲缘关系进程间的通信，允许一个进程和另一个与它有共同祖先的进程之间进行通信。
2. 命名管道（named pipe）:命名管道克服了管道没有名字的限制，因此，除具有管道所具有的功能外，它还允许无亲缘关 系 进程间的通信。命名管道在文件系统中有对应的文件名。命名管道通过命令mkfifo或系统调用mkfifo来创建。
3. 信号（Signal）:信号是比较复杂的通信方式，用于通知接受进程有某种事件发生，除了用于进程间通信外，进程还可以发送 信号给进程本身；Linux除了支持Unix早期信号语义函数sigal外，还支持语义符合Posix.1标准的信号函数sigaction（实际上，该函数是基于BSD的，BSD为了实现可靠信号机制，又能够统一对外接口，用sigaction函数重新实现了signal函数）。
4. 消息（Message）队列 :消息队列是消息的链接表，包括Posix消息队列system V消息队列。有足够权限的进程可以向队列中添加消息，被赋予读权限的进程则可以读走队列中的消息。消息队列克服了信号承载信息量少，管道只能承载无格式字节流以及缓冲区大小受限等缺
5. 共享内存 ：使得多个进程可以访问同一块内存空间，是最快的可用IPC形式。是针对其他通信机制运行效率较低而设计的。往往与其它通信机制，如信号量结合使用，来达到进程间的同步及互斥。
6. 内存映射（mapped memory）:内存映射允许任何多个进程间通信，每一个使用该机制的进程通过把一个共享的文件映射到自己的进程地址空间来实现它。
7. 信号量（semaphore）:主要作为进程间以及同一进程不同线程之间的同步手段。
8. 套接口（Socket）:更为一般的进程间通信机制，可用于不同机器之间的进程间通信。起初是由Unix系统的BSD分支开发出来的，但现在一般可以移植到其它类Unix系统上：linux和System V的变种都支持套接字。

#### 进程操作

Linux进程结构可由三部分组成：

* 代码段（程序）
* 数据段（数据）
* 堆栈段（控制块PCB）

进程控制块是进程存在的惟一标识，系统通过PCB的存在而感知进程的存在。系统通过 PCB 对进程进行管理和调度。PCB 包括创建进程、执行进程、退出进程以及改变进程的优先级等。

一般程序转换为进程分以下几个步骤：

1. 内核将程序读入内存，为程序分配内存空间
2. 内核为该进程分配进程标识符 PID 和其他所需资源
3. 内核为进程保存 PID 及相应的状态信息，把进程放到运行队列中等待执行，程序转化为进程后可以被操作系统的调度程序调度执行了

在 UNIX 里，除了进程 0（即 PID=0 的交换进程，Swapper Process）以外的所有进程都是由其他进程使用系统调用 fork 创建的，这里调用 fork 创建新进程的进程即为父进程，而相对应的为其创建出的进程则为子进程，因而除了进程 0 以外的进程都只有一个父进程，但一个进程可以有多个子进程。操作系统内核以进程标识符（Process Identifier，即 PID ）来识别进程。进程 0 是系统引导时创建的一个特殊进程，在其调用 fork 创建出一个子进程（即 PID=1 的进程 1，又称 init）后，进程 0 就转为交换进程（有时也被称为空闲进程），而进程1（init进程）就是系统里其他所有进程的祖先。

进程0：Linux引导中创建的第一个进程，完成加载系统后，演变为进程调度、交换及存储管理进程。 　　进程1：init 进程，由0进程创建，完成系统的初始化. 是系统中所有其它用户进程的祖先进程。

Linux中 1 号进程是由 0 号进程来创建的，因此必须要知道的是如何创建 0 号进程，由于在创建进程时，程序一直运行在内核态，而进程运行在用户态，因此创建 0 号进程涉及到特权级的变化，即从特权级 0 变到特权级 3，Linux 是通过模拟中断返回来实现特权级的变化以及创建 0 号进程，通过将 0 号进程的代码段选择子以及程序计数器EIP直接压入内核态堆栈，然后利用 iret 汇编指令中断返回跳转到 0 号进程运行。

* 创建一个进程

进程是系统中基本的执行单位。Linux 系统允许任何一个用户进程创建一个子进程，创建成功后，子进程存在于系统之中，并且独立于父进程。该子进程可以接受系统调度，可以得到分配的系统资源。系统也可以检测到子进程的存在，并且赋予它与父进程同样的权利。

Linux系统下使用 fork() 函数创建一个子进程，其函数原型如下：
```c
#include <unistd.h>
pid_t fork(void);
```
在讨论 fork() 函数之前，有必要先明确父进程和子进程两个概念。除了 0 号进程（该进程是系统自举时由系统创建的）以外，Linux 系统中的任何一个进程都是由其他进程创建的。创建新进程的进程，即调用 fork() 函数的进程就是父进程，而新创建的进程就是子进程。

fork() 函数不需要参数，返回值是一个进程标识符 (PID)。对于返回值，有以下 3 种情况：

1. 对于父进程，fork() 函数返回新创建的子进程的 ID。

2. 对于子进程，fork() 函数返回 0。由于系统的 0 号进程是内核进程，所以子进程的进程标识符不会是0，由此可以用来区别父进程和子进程。

3. 如果创建出错，则 fork() 函数返回 -1。

fork() 函数会创建一个新的进程，并从内核中为此进程分配一个新的可用的进程标识符 (PID)，之后，为这个新进程分配进程空间，并将父进程的进程空间中的内容复制到子进程的进程空间中，包括父进程的数据段和堆栈段，并且和父进程共享代码段（写时复制）。这时候，系统中又多了一个进程，这个进程和父进程一模一样，两个进程都要接受系统的调度。

<p align="center">
<img width="500" align="center" src="../images/33.jpg" />
</p>

下面给出的示例程序用来创建一个子进程，该程序在父进程和子进程中分别输出不同的内容。
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main(void){
	pid_t pid; // 保存进程ID
	pid = fork(); // 创建一个新进程
	if(pid < 0){ // fork出错
		printf("fail to fork\n");
		exit(1);
	}else if(pid == 0){	// 子进程
        // 打印子进程的进程ID
		printf("this is child, pid is : %u\n", getpid()); 
	}else{
        // 打印父进程和其子进程的进程ID
		printf("this is parent, pid is : %u, child-pid is : %u\n", getpid(), pid);	
	}
	return 0;
}
```
运行 :
```c
> ./fork 
Parent, PID: 2598, Sub-process PID: 2599
Sub-process, PID: 2599, PPID: 2598
```
由于创建的新进程和父进程在系统看来是地位平等的两个进程，所以运行机会也是一样的，我们不能够对其执行先后顺序进行假设，先执行哪一个进程取决于系统的调度算法。如果想要指定运行的顺序，则需要执行额外的操作。正因为如此，程序在运行时并不能保证输出顺序和上面所描述的一致。

getpid() 是获得当前进程的pid，而 getppid() 则是获得父进程的 id。

* 父子进程的共享资源

子进程完全复制了父进程的地址空间的内容，包括堆栈段和数据段的内容。子进程并没有复制代码段，而是和父进程共用代码段。这样做是存在其合理依据的，因为子进程可能执行不同的流程，那么就会改变数据段和堆栈段，因此需要分开存储父子进程各自的数据段和堆栈段。但是代码段是只读的，不存在被修改的问题，因此这一个段可以让父子进程共享，以节省存储空间.

<p align="center">
<img width="500" align="center" src="../images/34.jpg" />
</p>

该程序定义了一个全局变量 global、一个局部变量 stack 和一个指针 heap。该指针用来指向一块动态分配的内存区域。之后，该程序创建一个子进程，在子进程中修改 global、stack 和动态分配的内存中变量的值。然后在父子进程中分别打印出这些变量的值。由于父子进程的运行顺序是不确定的，因此我们先让父进程额外休眠2秒，以保证子进程先运行。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
// 全局变量，在数据段中
int global; 
int main(){
	pid_t pid;
	int stack = 1; // 局部变量，在栈中
	int * heap;
	heap = (int *)malloc(sizeof(int)); // 动态分配的内存，在堆中
	*heap = 2;
	pid = fork(); // 创建一个子进程
	if(pid < 0){ // 创建子进程失败
		printf("fail to fork\n");
		exit(1);	
	}else if(pid == 0){ // 子进程，改变各变量的值
		global++; // 修改栈、堆和数据段
		stack++;
		(*heap)++;
		printf("the child, data : %d, stack : %d, heap : %d\n", global, stack, *heap);
		exit(0); // 子进程运行结束
	}
       // 父进程休眠2秒钟，保证子进程先运行
	sleep(2); 
       // 输出结果
	printf("the parent, data : %d, stack : %d, heap : %d\n", global, stack, *heap);
	return 0;
}
```

程序运行效果如下：
```c
> ./fork 
In sub-process, global: 2, stack: 2, heap: 3
In parent-process, global: 1, stack: 1, heap: 2
```
由于父进程休眠了2秒钟，子进程先于父进程运行，因此会先在子进程中修改数据段和堆栈段中的内容。因此不难看出，子进程对这些数据段和堆栈段中内容的修改并不会影响到父进程的进程环境。

* fork()函数的出错情况

有两种情况可能会导致fork()函数出错：

1. 系统中已经有太多的进程存在了.

2. 调用fork()函数的用户进程太多了.

一般情况下，系统都会对一个用户所创建的进程数加以限制。如果操作系统不对其加限制，那么恶意用户可以利用这一缺陷攻击系统。下面是一个利用进程的特性编写的一个病毒程序，该程序是一个死循环，在循环中不断调用fork()函数来创建子进程，直到系统中不能容纳如此多的进程而崩溃为止。

<p align="center">
<img width="500" align="center" src="../images/35.jpg" />
</p>

```c
#include <unistd.h>
int main(){
	while(1)
		fork(); /* 不断地创建子进程，使系统中进程溢满 */
	return 0;
}
```
* 创建共享空间的子进程

进程在创建一个新的子进程之后，子进程的地址空间完全和父进程分开。父子进程是两个独立的进程，接受系统调度和分配系统资源的机会均等，因此父进程和子进程更像是一对兄弟。如果父子进程共用父进程的地址空间，则子进程就不是独立于父进程的。

Linux环境下提供了一个与 fork() 函数类似的函数，也可以用来创建一个子进程，只不过新进程与父进程共用父进程的地址空间，其函数原型如下：
```c
#include <unistd.h>
pid_t vfork(void);
```

vfork() 和 fork() 函数的区别如下：

1. vfork() 函数产生的子进程和父进程完全共享地址空间，包括代码段、数据段和堆栈段，子进程对这些共享资源所做的修改，可以影响到父进程。由此可知，vfork() 函数与其说是产生了一个进程，还不如说是产生了一个线程。

2. vfork() 函数产生的子进程一定比父进程先运行，也就是说父进程调用了 vfork() 函数后会等待子进程运行后再运行。

在子进程中，我们先让其休眠 2 秒以释放 CPU 控制权，在前面的 fork() 示例代码中我们已经知道这样会导致其他线程先运行，也就是说如果休眠后父进程先运行的话，则第 1 点则为假；否则为真。第 2 点为真，则会先执行子进程，那么全局变量便会被修改，如果第 1 点为真，那么后执行的父进程也会输出与子进程相同的内容。

```c
//@file vfork.c
//@brief vfork() usage
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int global = 1;

int main(void){
    pid_t pid;
    int   stack = 1;
    int  *heap;

    heap = (int *)malloc(sizeof(int));
    *heap = 1;

    pid = vfork();
    if (pid < 0){
        perror("fail to vfork");
        exit(-1);
    }else if (pid == 0) {
        //sub-process, change values
        sleep(2);//release cpu controlling
        global = 999;
        stack  = 888;
        *heap  = 777;
        //print all values
        printf("In sub-process, global: %d, stack: %d, heap: %d\n",global,stack,*heap);
        exit(0);
    }else{
        //parent-process
        printf("In parent-process, global: %d, stack: %d, heap: %d\n",global,stack,*heap);
    }

    return 0;
}
```
运行
```c
> ./vfork 
In sub-process, global: 999, stack: 888, heap: 777
In parent-process, global: 999, stack: 888, heap: 777
```
* 在函数内部调用vfork

在使用 vfork() 函数时应该注意不要在任何函数中调用 vfork() 函数。下面的示例是在一个非 main 函数中调用了 vfork() 函数。该程序定义了一个函数 f1()，该函数内部调用了 vfork() 函数。之后，又定义了一个函数 f2()，这个函数没有实际的意义，只是用来覆盖函数 f1() 调用时的栈帧。main 函数中先调用 f1() 函数，接着调用 f2() 函数。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int f1(void){
    vfork();
    return 0;
}

int f2(int a, int b){
    return a+b;
}

int main(void){
    int c;
    
    f1();
    c = f2(1,2);
    printf("%d\n",c);

    return 0;
}
```
运行:
```c
> ./vfork 
3
Segmentation fault (core dumped)
```
通过运行结果可以看出，一个进程运行正常，打印出了预期结果，而另一个进程似乎出了问题，发生了段错误。出现这种情况的原因可以用下图来分析一下：

<p align="center">
<img width="500" align="center" src="../images/36.jpg" />
</p>
调用 vfork() 之后产生了一个子进程，并且和父进程共享堆栈段，两个进程都要从 f1() 函数返回。由于子进程先于父进程运行，所以子进程先从 f1() 函数中返回，并且调用 f2() 函数，其栈帧覆盖了原来 f1() 函数的栈帧。当子进程运行结束，父进程开始运行时，就出现了右图的情景，父进程需要从 f1() 函数返回，但是 f1() 函数的栈帧已经被 f2() 函数的所替代，因此就会出现父进程返回出错，发生段错误的情况。

由此可知，使用 vfork() 函数之后，子进程对父进程的影响是巨大的，其同步措施势在必行。