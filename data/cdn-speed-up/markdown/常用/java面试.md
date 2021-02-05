## 1.0 java面试
### 1.1 ReentrantLock与
```
可重入、锁超时、可中断、公平锁
共同点：
    1. 都是用来协调多线程对共享对象、变量的访问
    2. 都是可重入锁，同一线程可以多次获得同一个锁
    3. 都保证了可见性和互斥性
不同点：
    1. ReentrantLock 显示的获得、释放锁，synchronized 隐式获得释放锁
    2. ReentrantLock 可响应中断、可轮回，synchronized 是不可以响应中断的，为处理锁的不可用性提供了更高的灵活性
    3. ReentrantLock 是 API 级别的，synchronized 是 JVM 级别的
    4. ReentrantLock 可以实现公平锁
    5. ReentrantLock 通过 Condition 可以绑定多个条件
    6. 底层实现不一样， synchronized 是同步阻塞，使用的是悲观并发策略，lock 是同步非阻塞，采用的是乐观并发策略
    7. Lock 是一个接口，而 synchronized 是 Java 中的关键字，synchronized 是内置的语言实现。
    8. synchronized 在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而 Lock 在发生异常时，如果没有主动通过 unLock()去释放锁，则很可能造成死锁现象，因此使用 Lock 时需要在 finally 块中释放锁。
    9. Lock 可以让等待锁的线程响应中断，而 synchronized 却不行，使用 synchronized 时，等待的线程会一直等待下去，不能够响应中断。
    10. 通过 Lock 可以知道有没有成功获取锁，而 synchronized 却无法办到。
    11. Lock 可以提高多个线程进行读操作的效率，既就是实现读写锁等
```


### 1.2 链表反转
```

public class ReverseList {

    public static void main(String[] args) {
        ListNode listNode = create();
        listNode = reverseList(listNode);
        System.out.println("0000000000");
    }

    public static ListNode create() {
        ListNode node01 = new ListNode("A");
        ListNode node02 = new ListNode("B");
        ListNode node03 = new ListNode("C");
        ListNode node04 = new ListNode("D");
        ListNode node05 = new ListNode("E");
        ListNode node06 = new ListNode("F");
        node01.next = node02;
        node02.next = node03;
        node03.next = node04;
        node04.next = node05;
        node05.next = node06;
        return node01;
    }

    public static ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode next = null;
        while (head != null) {
            next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }

    public static class ListNode {
        private String data;
        private ListNode next;

        public ListNode() { }

        public ListNode(String data) {
            this.data = data;
        }
    }
}

```

### 1.3 快排
```
public class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low>=high) return;
        int start = low;
        int end = high;
        int temp = arr[low];
        while (start<end) {
            while ((start<end) && arr[end]>=temp) {
                end--;
            }
            while ((start<end) && temp>=arr[start]) {
                start++;
            }
            if (start<end) {
                int t = arr[start];
                arr[start] = arr[end];
                arr[end] = t;
            }
        }
        arr[low] = arr[start];
        arr[start] = temp;
        quickSort(arr, low, start-1);
        quickSort(arr, start+1, high);
    }


    public static void main(String[] args) {
        int[] arr = {10, 7, 2, 4, 7, 62, 3, 4, 2, 1, 8, 9, 19};
        quickSort(arr, 0, arr.length - 1);
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
}
```

### 1.4 Map的知识
```
1.数据结构
    JDK1.7:Segment数组+HashEntry数组+链表
    JDK1.8之后：Node数组+链表+红黑树
2.初始化长度为16，加载因子为0.75的集合
3.添加key和value过程
    01) 先判断put的值的key是否为null，如果为null，会固定存放到table[0]下面
    02) 会通过hash(()方法算出key的hash地址，通过hash地址取模找到数组下标索引
```


### 1.5 MyISAM与InnoDB的区别
```
1. InnoDB支持事务，MyISAM不支持事务； 
2. InnoDB支持外键，而MyISAM不支持外键。对一个包含外键的InnoDB表转为MYISAM会失败； 
3. InnoDB是聚集索引，使用B+Tree作为索引结构，数据文件是和（主键）索引绑在一起的，必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。
    MyISAM是非聚集索引，也是使用B+Tree作为索引结构，索引和数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
    也就是说：InnoDB的B+树主键索引的叶子节点就是数据文件，辅助索引的叶子节点是主键的值；而MyISAM的B+树主键索引和辅助索引的叶子节点都是数据文件的地址指针。
4. InnoDB支持表、行(默认)级锁，而MyISAM支持表级锁
    InnoDB的行锁是实现在索引上的，而不是锁在物理行记录上。潜台词是，如果访问没有命中索引，也无法使用行锁，将要退化为表锁。
    t_user(uid 主键, uname, age, sex) innodb;
    update t_user set age=10 where uid=1;              命中索引，行锁。
    update t_user set age=10 where uid!= 1;            未命中索引，表锁。
    update t_user set age=10 where name='chackca';     无索引，表锁。
5. InnoDB不保存表的具体行数，执行select count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快（注意不能加有任何WHERE条件）；
    因为InnoDB的事务特性，在同一时刻表中的行数对于不同的事务而言是不一样的，因此count统计会计算对于当前事务而言可以统计到的行数，而不是将总行数储存起来方便快速查询。InnoDB会尝试遍历一个尽可能小的索引除非优化器提示使用别的索引。如果二级索引不存在，InnoDB还会尝试去遍历其他聚簇索引。
    如果索引并没有完全处于InnoDB维护的缓冲区（Buffer Pool）中，count操作会比较费时。可以建立一个记录总行数的表并让你的程序在INSERT/DELETE时更新对应的数据。和上面提到的问题一样，如果此时存在多个事务的话这种方案也不太好用。如果得到大致的行数值已经足够满足需求可以尝试SHOW TABLE STATUS
6、InnoDB表必须有唯一索引（如主键）（用户没有指定的话会自己找/生产一个隐藏列Row_id来充当默认主键），而Myisam可以没有
7、Innodb存储文件有frm、ibd，而Myisam是frm、MYD、MYI
    Innodb：frm是表定义文件，ibd是数据文件
    Myisam：frm是表定义文件，myd是数据文件，myi是索引文件
```


### 1.6 dubbo支持的9种协议
* Dubbo支持dubbo、rmi、hessian、http、webservice、thrift、memcached、redis、rest这9种协议，默认使用dubbo协议

### 1.7 dubbo有多少负载均衡策略
* 随机调用(默认)
* 轮询
* 最少活跃调用数

* 理论上1个服务提供者需要20个服务消费者才能压满网卡

### 1.8 ThreadPoolExecutor的4种拒绝策略
* ThreadPoolExecutor.AbortPolicy策略，RejectedExecutionHandler handler = new ThreadPoolExecutor.AbortPolicy(); 抛出 RejectedExecutionException 异常
* ThreadPoolExecutor.CallerRunsPolicy策略，RejectedExecutionHandler handler = new ThreadPoolExecutor.CallerRunsPolicy(); 用于被拒绝任务的处理程序，它直接在 execute 方法的调用线程中运行被拒绝的任务；如果执行程序已关闭，则会丢弃该任务。
* ThreadPoolExecutor.DiscardOldestPolicy策略，丢弃任务队列中最旧任务，丢弃最旧任务也不是简单的丢弃最旧的任务，而是有一些额外的处理。
* ThreadPoolExecutor.DiscardPolicy策略，丢弃将要加入队列的任务本身

### 1.9 线程的五种状态
线程一共有以下几种状态：
1、新建状态(New)：新创建了一个线程对象。
2、就绪状态(Runnable)：线程对象创建后，其他线程调用了该对象的start()方法。该状态的线程位于“可运行线程池”中，变得可运行，只等待获取CPU的使用权。即在就绪状态的进程除CPU之外，其它的运行所需资源都已全部获得。
3、运行状态(Running)：就绪状态的线程获取了CPU，执行程序代码。
4、阻塞状态(Blocked)：阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。
    阻塞的情况分三种：
    (1)、等待阻塞：运行的线程执行wait()方法，该线程会释放占用的所有资源，JVM会把该线程放入“等待池”中。进入这个状态后，是不能自动唤醒的，必须依靠其他线程调用notify()或notifyAll()方法才能被唤醒，
    (2)、同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入“锁池”中。
    (3)、其他阻塞：运行的线程执行sleep()或join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。
5、死亡状态(Dead)：线程执行完了或者因异常退出了run()方法，该线程结束生命周期。


### 1.10 spring事务七种传播属性
```
PROPAGATION_REQUIRED -- 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。
PROPAGATION_SUPPORTS -- 支持当前事务，如果当前没有事务，就以非事务方式执行。
PROPAGATION_MANDATORY -- 支持当前事务，如果当前没有事务，就抛出异常。
PROPAGATION_REQUIRES_NEW -- 新建事务，如果当前存在事务，把当前事务挂起。
PROPAGATION_NOT_SUPPORTED -- 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
PROPAGATION_NEVER -- 以非事务方式执行，如果当前存在事务，则抛出异常。
PROPAGATION_NESTED -- 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。
```

### 1.11 类加载机制
```
001) 加载
类加载过程的一个阶段，ClassLoader通过一个类的完全限定名查找此类字节码文件，并利用字节码文件创建一个class对象。
002) 验证
目的在于确保class文件的字节流中包含信息符合当前虚拟机要求，不会危害虚拟机自身的安全，主要包括四种验证：文件格式的验证，元数据的验证，字节码验证，符号引用验证。
003) 准备
为类变量（static修饰的字段变量）分配内存并且设置该类变量的初始值，（如static int i = 5 这里只是将 i 赋值为0，在初始化的阶段再把 i 赋值为5)，这里不包含final修饰的static ，因为final在编译的时候就已经分配了。这里不会为实例变量分配初始化，类变量会分配在方法区中，实例变量会随着对象分配到Java堆中。
004) 解析
这里主要的任务是把常量池中的符号引用替换成直接引用
005) 初始化
这里是类记载的最后阶段，如果该类具有父类就进行对父类进行初始化，执行其静态初始化器（静态代码块）和静态初始化成员变量。（前面已经对static 初始化了默认值，这里我们对它进行赋值，成员变量也将被初始化）
```

CAS是通过调用循环调用compareAndSet来判断值是有修改，通过调用unsafe.compareAndSwapInt来实现，内部调用Ｃ++代码并且加锁（多处理器），通过offset读取内存中指定位置的数值，再跟当前高速缓存中的值比较是否一致。