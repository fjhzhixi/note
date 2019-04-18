# wait+notify

任何一个对象都有一个等待线程队列,其均有`wait + notify`模式

**任何一个共享对象都有一个`entry`和`wait`队列,`entry`队列是所有共享该对象的可执行状态的线程,`wait`队列中是调用`wait`方法之后加入的,每次从等待队列中唤醒的线程和进入队列中的线程一起竞争共享对象使用权**

对于共享它的所有线程 : 

* `object.wait`方法 : **调用的前提是该线程持有该object对象的一个锁**,在该线程中调用`object.wait`将线程放入`object`的等待队列,当前线程会暂停运行,**并且进入等待队列的线程不可以被调度**,**并释放其实例的锁**
* `object.notify(All)方法` : **调用的前提是该线程持有锁**,唤醒`object`的等待队列中的任意一个(所有)线程,**所谓唤醒指的不是立即调度执行,而是将其从等待队列中取出,改变线程为可执行状态,然后等待被调用**
* 注 ：执行`notify`后的线程状态唤醒后的线程并不会立即运行,**因为调用`notify`方法的线程还持有锁,在其还未结束之前其他线程无法获得锁**

# Single Threaded Execution

同一时间只允许一个线程访问某个共享对象

## 使用情形:

1. 多线程共享
2. 状态可能发生改变
3. 有确保安全性的需求

## 实现 

##  **对对象加锁**

* 使用锁的注意事项 :

  1. 在`{`处对对象加锁,在`}`处释放对象的锁

  2. 当在语句块中使用`lock(), unlock()`时:

     * **当`{}`语句块中存在`return`时可能不会释放锁,即中途返回不会释放在开始拿到的锁**

     * **当在方法中抛出异常时可能不会释放锁**

  3. 使用`synchronized`时不会出现上述问题

  4. 锁保护着什么 : 保护着其语句块的执行逻辑

  5. 使用那个锁保护 : 一般都是使用该方法所在的**共享对象**的锁来保护

     ​			也可以通过一个属性中的对象专门作为锁(为逻辑服务)

# Immutable

**不可变对象属性不能被修改,所以一定是线程安全的,无需显式加锁**

## 使用情形 :

1. 实例创建后状态不再改变
2. **实例是共享的,且被频繁访问**

## 实现 :

1. 对所有的属性均设置为`private final`形式的,即一旦创建初始化之后再也无法改变.
2. 只配置`get`方法,不能配置`set`方法
3. 可以考虑成对的`mutable`类与`immutable`类,即可以分开使用`set`与不使用的情形,这时可以配置成对的可变对象与不可变对象,并且二者可以相互创建

# Guarded Suspension

## 使用模式

```java
while(/*守护条件的逻辑非*/) {
    //使用wait进行等待守护条件成立
}
// 执行目标处理语句
```

## 示例

```java
public synchronized Request getRequest() {
    while (queue.peek() == null) {
        try {
            wait();
        } catch(Excepetion e){};
    }
    return queue.remove();
}

public synchronized void putRequest(Request request) {
    queue.offer(request);
    notifyAll();
}
```

1. 目标执行语句`queue.remove()`从请求队列中取出一项
2. **该目标执行语句的守护条件是队列不为空**, 使用`while`结构块的作用是**保证在执行取出时队列一定不为空**
3. 在不满足守护条件是进入等待队列,等待守护条件满足并且同时被唤醒之后执行目标语句

## 实现

实现一个守护类`Obj`,守护的是获取操作

1. 一个线程从`Obj`中获取信息,该获取方法使用`while() + wait()`来确定满足获取操作的守护条件
2. 一个线程改变`Obj`的状态使得满足获取操作的守护条件

# Balking

## 使用情形 :

如果现在不适合执行这个操作,或者没必要执行这个操作,就停止处理,直接返回

1. 语句并不一定要执行 : 可以提高性能
2. 不需要等待守护条件成立 : 当检测到守护条件不成立时,立即返回进行下一个操作

## 实现

1. 被防护的对象中拥有一个被防护的方法,当线程执行该方法时,若守护条件成立则执行实际操作,否则直接返回,**守护条件是否成立与对象的状态有关**

# 介于Balking,Guarded Suspension之间

* 在`Balking`中守护条件一旦不满足则线程立即返回退出
* 在`Guarded Suspension`守护条件不满足时线程会`wait()`直到条件满足
* **取一个折中的设计 : 当`wait()`一定时间之后不满足则退出, 并且要区分是超时退出还是执行完退出**

```java
public class Host {
    private final long timeout;	// 超时时间
    private boolean ready = false; 	// 守护条件是否满足
    // 修改状态
    public synchronized void setStatus(boolean status) {
        ready = status;
        notifyAll();
    }
    // 检测状态后执行,超时直接退出
    public synchronized void excute() {
        long start = System.currentTimeMillis();
        while(!ready) {
            long now = System.currentTimeMillis();
            long rest = timeout - (now - start);
            if (rest <= 0) {
                // 超时
                return;
            }
            wait(rest);
        }
        // do excute : 真正的目标逻辑代码
    }
}
```

# Interrupt

## throws InterruptedException

抛出中断异常的方法有`wait(), sleep(), join()`,其共同特点有:

1. 花费时间
2. 可以取消

**`interrupt()`方法只是改变了线程的中断状态,只有线程中有检测状态的语句时才能抛出中断异常**

### sleep&interrupt

当`Thread1`处于`sleep`状态时,可以在一个可以执行的线程中执行`Thread1.interrupt()`使其终止暂停状态并抛出中断异常

无论何时,任何线程都能调用其他线程的`interrupt`方法

### wait&interrupt

当对在等待对列中的线程执行`interrrupt`方法时,其会从等待对列中出来,在重新获得锁之后抛出中断异常

### join&interrupt

当线程使用`join`方法等待其他线程终止时,可以使用`interrupt`方法取消等待

**上述所有抛出中断异常之后线程控制权均交到`catch`块中**

# Read-Write Lock

## 使用情景

1. 多个线程同时读取没有冲突(读是安全的操作,不会改变属性)
2. 多个线程同时写入会有冲突(写是不安全的操作,必须保证操作的原子性)
3. 两个或以上线程同时读和写会有冲突

即冲突分析 :

* 当线程试图获得读取的锁时 :
  * 如果有线程在执行写入,则等待
  * 如果有线程在执行读取,无需等待
* 当线程试图获取写入的锁时 :
  * 如果有线程在执行写入,则等待
  * 如果有线程在执行读取,则等待

作用 :

1. 读取并行来提高程序性能
2. 适合读取操作繁重时
3. 适合读取频率比写入的频率高

## 实现

**核心在于自定义一个锁类 : `ReadWriteLock`**

**`java`提供的`synchronized`是一种物理锁,不能认为改变机制,但是我们可以实现逻辑锁来实现一些特定功能**

```java
public final class ReadWriteLock {
    private int readingReaders = 0;	//正在读取的线程个数
    private int waitingWriters = 0;	//正在等待写入的线程个数
    private int writingWriters = 0; //正在写入的线程个数
    private boolean preferWriter = true; //为真时写入优先
    // 对读取方法的上锁
    public synchronized void readLock() throws InterruptedException {
        /* 等待条件 :
           1. 当前有正在写入的线程
           或
           2. 当前有正在等待写入的线程,且写入优先级
        */
        while (writingWriters > 0 || (preferWriter && waitingWriters) > 0) {
            wait();
        }
        // 该线程进行读入
        readingReaders++;
    }
    // 对读取方法解锁
    public synchronized void readUnlock() {
        readingReaders--;
        // 在不进行读取时写入优先
        preferWriter = true;
        notifyAll();
    }
    
    
    // 对写入方法加锁
    public synchronized void writeLock() throws InterruptedException {
        // 正在等待写入的线程+1
        waitingWriters++;
        /* 等待条件
           1. 当前有正在读取的线程
           或
           2. 当前有正在写入的线程
        */
        try {
            while (readingReaders > 0 || writingWriters > 0) {
                wait();
            }
        } finally {
                // 无论是等待到唤醒还是抛出中断异常,等待队列都要-1
                waitingWriters--;
        }
            // 等待结束正在写入的线程加1
        writingWriters++;
    }
    // 对写入方法解锁
    public synchronized void writeUnlock() {
        writingWriters--;
        preferWriter = false;
        notifyAll();
    }
}
```

**对共享数据使用读写锁进行保护**

```java
public class Data {
    // 存储数据的数据结构
    private final char[] buffer;
    private final ReadWriteLock lock = new ReadWriteLock();
    
    // 线程从数据中读取的安全方法
    public char[] read() throws InterruptedException {
        lock.readLock();
        try {
            return doRead();	// 真正的读入数据方法
        } finally {
            lock.readUnlock();	// 无论是否抛出异常都要解锁
        }
    }
    // 线程向数据中写入的安全方法
    public void write(char c) throws InterruptedException {
        lock.writeLock();
        try {
            doWrite();		// 真正的写入方法
        }finally {
            lock.writeUnlock();
        }
    }
}
```

# Thread-Per-Message

## 使用情形

1. 提高响应,**要求请求没有返回值**,即委托线程可以继续执行,而执行端执行即可
2. 要求请求的操作顺序没有要求

## 实现

* 核心在于委托端线程,执行端线程,委托端通过发送消息让执行端执行,执行端对每一个请求新开一个线程来处理,**但是只负责启动线程即可(`start`),之后控制权便返回委托方,具体处理线程自己执行**

# Worker Thread

## 角色

* `委托者Client(Thread)` ：

  创建请求并将其传递给`Channel`对象中的工作队列

* `通信线路(Channel)` ：

  由委托者和工人共享的线程安全对象，有着放入请求，拿出请求的功能

* `工人Worker(Thread)` ：

  不断从从`Channel`中拿出请求来执行

* `请求Request` ：

  保存了一项工作的必要信息

## 模式的作用

* 通过自定义`worker`的数目实现控制吞吐量
* 使用`channel`的缓冲队列匹配委托者与工人的效率的差异
* 调用与执行分离



