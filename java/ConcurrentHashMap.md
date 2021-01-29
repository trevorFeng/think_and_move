### 1.7分析
- 使用Segment数组，继承了ReentrantLock，一个Segment就是一个hash子表
- 并发环境下，对于不同Segment的数据进行操作是不用考虑锁竞争的
- 实际上就是降低了锁的粒度
- 扩容的时候，会对Segment加锁，所以仅仅影响这个Segment，不同的Segment还是可以并发的，所以解决了线程的安全问题

### 1.8分析
- 使用NODE数组+链表+红黑树实现
- 使用Synchronized和CAS来操作来控制数据安全

### 1.8 put()方法
```
根据 key 计算出 hashcode 。

判断是否需要进行初始化。

f 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。

如果当前位置的 hashcode == MOVED == -1,则需要进行扩容。

如果都不满足，则利用 synchronized 锁写入数据。

如果数量大于 TREEIFY_THRESHOLD 则要转换为红黑树

```

### 1.8扩容机制
