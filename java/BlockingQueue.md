### 阻塞队列
- 它是基于ReentrantLock和condition实现

### LinkedBlockingQueue
- 使用两把锁。一把用于入队，一把用于出队
- 入队时根据方法的不同可以选择阻塞，立即返回，等待超时
```
offer(E e)：如果队列没满，立即返回true； 如果队列满了，立即返回false-->不阻塞

put(E e)：如果队列满了，一直阻塞，直到队列不满了或者线程被中断-->阻塞

offer(E e, long timeout, TimeUnit unit)：在队尾插入一个元素,，如果队列已满，则进入等待，直到出现以下三种情况：-->阻塞

被唤醒

等待时间超时

当前线程被中断
```
- 出队同理
- AtomicInteger的一个count实现计数

### ArrayBlockingQueue
- 使用一把锁,

### DelayQueue 
并发安全的延时队列，每个元素都有过期时间，只有过期元素才会出队列，队列中的元素是按过期时间排序的

