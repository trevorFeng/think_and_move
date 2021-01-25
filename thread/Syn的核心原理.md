### java对象内存布局
![](../a-imgs/对象头.png)
- 对象头
    - Mark Word 
        - 对象的hashCode
        - 锁信息
        - 分代年龄
        - GC标志等
    - Class Metadata Address
        - 类型指针指向对象的类元数据，JVM通过这个指针确定该对象是哪个类的实例
- 实例变量
- 填充数据

### mark word里的锁信息
- 重量级锁，指向与之关联的monitor对象的指针
- 偏向锁，存放线程ID
- 轻量级锁，指向栈中记录的指针

##### monitor数据结构
```C++
ObjectMonitor() {
    _header       = NULL;
    _count        = 0; //记录个数
    _waiters      = 0,
    _recursions   = 0;
    _object       = NULL;
    _owner        = NULL;
    _WaitSet      = NULL; //处于wait状态的线程，会被加入到_WaitSet
    _WaitSetLock  = 0 ;
    _Responsible  = NULL ;
    _succ         = NULL ;
    _cxq          = NULL ;
    FreeNext      = NULL ;
    _EntryList    = NULL ; //处于等待锁block状态的线程，会被加入到该列表
    _SpinFreq     = 0 ;
    _SpinClock    = 0 ;
    OwnerIsThread = 0 ;
  }
```
- 每个线程都会封装成ObjectWaiter对象
- _WaitSet和_EntryList用来保存ObjectWaiter对象
- 多个线程访问一段同步代码，会进入_EntryList集合，当线程获取到对象的monitor对象后，会将woner设置为当前线程ID，并且count加一（可重入）
- 当线程调用wait方法，woner重新为null并且加入WaitSet集合中

##### 代码块同步
```java
3: monitorenter  //进入同步方法
//..........省略其他  
15: monitorexit   //退出同步方法
16: goto          24
//省略其他.......
21: monitorexit //退出同步方法
```

##### 方法同步
JVM可以从方法常量池中的方法表结构(method_info Structure) 中的 ACC_SYNCHRONIZED 访问标志区分一个方法是否同步方法

如果ACC_SYNCHRONIZED被设置了
- 执行线程将先持有monitor
- 执行完毕或者抛出异常释放monitor