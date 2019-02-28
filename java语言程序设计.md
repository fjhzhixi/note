# 9. 对象和类

类为对象定义属性和行为

## 类的内容

1. 一个对象的状态(固有属性):由其数据域表示
2. 一个对象可以进行的行为(动作):由其定义的方法实现

## 类的构造方法

与**类名**相同,`public`修饰,**无返回类型**

## 静态方法

1. 一般数据和方法属于不同的类的实例对象,在堆栈中分配有不同的空间
2. 静态的`static`数据和方法被类的所以实例对象共享,只占据一块内存空间

## 可见性修饰符

| 访问权限 |  类  |  包  | 子类 | 其他包 |
| :------: | :--: | :--: | :--: | :----: |
|  public  | 可以 | 可以 | 可以 |  可以  |
| protect  | 可以 | 可以 | 可以 |  不可  |
| default  | 可以 | 可以 | 不可 |  不可  |
| private  | 可以 | 不可 | 不可 |  不可  |

* 注意private配置set以及get方法可以用来进行对数据域的封装

## 不可变类

* 所有的数据域都是私有的
* 没有修改器方法
* 没有一个返回指向可变数据源的引用的访问器方法

# 10. 面向对象思考







# 19.泛型

## 泛型的目的

使得在编译而不是运行时检测出错误

## 泛型的使用

**泛型在实例化是被替换为具体的类型**

1. 定义泛型类和接口

   ```java
   public class GenericClass<E> {
       // 注意构造方法的定义时是没有E的
       public GenericClass(){
           
       }
       // 在之后方法的参数可以用泛型E来定义
   }
   ```

   在实例化时E要替换为具体的类型`GenericClass<Integer> IntClass = new GenericClass();`

2. 定义泛型方法

   `public static <E> void print(E[] list)`

   1. 泛型方法都为是静态方法
   2. 将泛型类型<E>放在返回值之前

## 泛型的限制

1. 不能使用`new E()`,即不能为泛型创建实例
2. 不能使用`new E[]`
3. **在静态的上下文中不允许类的参数是泛型类型**
4. 异常类不能是泛型的

## 通配泛型

通配泛型即为有一定限制的泛型类

1. 非受限通配:`?`	//等价于`? extends Object`
2. 受限通配:`? extends T` 只能匹配由T或T的子类实现的泛型类
3. 下限通配:`? super T` 只能匹配由T或T的父类实现的泛型类

---

# 20.Collection

## 基本概念

1. 合集(collection)是一个泛型接口,存储一个元素的集合
2. 具体分类
   1. Set:用于存储一组不重复的元素
   2. List:用于存储一组有序元素
   3. Stack:栈
   4. Queue:队列
   5. Priority Queue:优先队列

## List(线性表)

**主要有ArrayList和LinkedList两种**

### List类

List实现了双向遍历功能的迭代器

1. ArrayList: 当需要通过下标随机访问数据时使用
2. LinkList:当需要在线性表头尾频繁插入数据时使用

### Comparator接口/Comparable接口

只有实现了Comparable接口的类才可以比较大小,对于未实现的类要进行比较

**需要创建一个实现Comparator<T>接口的比较器类并重写它的compare方法**

```java
public class ObjectComparator implements Comparator<Object> {
    @ Override
        public int compare(Object o1, Objecr o2) {
        // 具体进行比较的代码
    }
}
```

对于实现g给了Comparable接口的类可以使用compareTo方法直接进行比较(按自然顺序)



# 30.多线程与并行程序设计

## 创建任务和线程

1. 一个任务类必须实现`Runnable`接口,其中至少包含构造方法与run方法

   ```java
   // 定义任务类
   public class TaskClass implements Runnable {
       public TaskClass() {
           
       }
       public void run() {
           
       }
   }
   // 创建任务对象
   TaskClass task = new TaskClass();
   ```

2. 任务必须由线程执行

   `Thread thread = new Thread(task);`

3. 调用线程的start方法运行线程,java虚拟机自动调用其任务的run方法

   `thread.start();`

**注意 : 定义任务的类最好在使用任务的类的外部定义, 将任务与线程分开**

## Thread类的控制

**对线程的控制实现是在任务的run()方法中通过Thread的静态方法**

1. `yield()`方法 : 引发一个线程暂停并允许其他线程运行

2. `sleep(mills:long)`方法 : 指定线程的休眠时间

   **sleep()方法要求由必检的异常**

   ```java
   public void run() {
       try {
           while() {
               ...
               Thread.sleep(1000);
           }
       }
       catch (InterruptedException ex) {
           ex.printStackTrace();
       }
   }
   ```

   **注意循环机制要放在try-catch块中**

3. `join()`方法等待一个线程的结束(同样需要检测异常)  **即可以使的该线程优先执行完**

4. **如果高优先级的线程一直不退出运行,则低优先级的线程则没有机会运行,所有高优先级的线程要定时调用sleep或者yield**

## 线程池

**为多个任务创建线程**

1. 创建线程池(**Executors**类的静态方法) 

   * `+ newFixedThreadPool(numberOfThreads : int) : ExecutorService`

     创建一个指定线程数目的线程池,**一个线程在当前任务完成时可以重用执行另一个任务**

   * `+ newCachedThreadPool() : ExecutorService`

     创建一个线程池,他会在执行任务时创建线程,**但是如果之前创建过的线程可用,则重用之前的线程**

2. 线程池执行任务

   `Executor`接口的`+ execute(Runnable object) : void`方法

3. 控制线程

   `ExectorService`接口的

   * ` + shutdown() : void`方法 : 关闭执行器,不再接受新任务,但已经接受的任务仍可以玩完成

   * `+ shutdownNow() : List<Runnable>` 方法 : 立即关闭执行器,返回未完成任务列表 

```java
ExecutorService executor = Executor.newFixedThreadPool(num);
executor.execute(new Runnable());
executor.shutdown();
```

## 线程的竞争与同步

**同步用来解决竞争**

1. 竞争产生的原因 : 多个线程以一种冲突的方式访问同一个公共资源时

2. 使用同步解决竞争

   1. 同步方法 :

      * ````java
        public synchronized void methodName();
        ````

      * 同步方法机制 : 如果一个线程调用一个实例对象(类)上的同步实例方法(静态方法),首先**给这个实例对象(类)加锁**,然后执行该方法,执行完后解锁,**在解锁之前,所有试图调用这个实例对象(类)的线程都会被阻塞**

   2. 同步语句:

      * ```java
        synchronized (expr) {
            statements;
        }
        ```

        **expr求值结果为一个对象的引用**

      * 同步语句机制:

        1. 若expr指向的类被其他线程上锁,则该线程阻塞直到解锁
        2. 若expr指向的类未被上锁,则该线程给其上锁,执行statements,之后解锁

      * 以下二者等价

        ```java
        public synchronized void methodName() {
            // method body
        }
        
        public void methodName() {
            synchronized (this) {
                // method body
            }
        }
        ```

        

      