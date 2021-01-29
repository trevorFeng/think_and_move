### TreeMap
- TreeMap 是一个有序的key-value集合，它是通过红黑树实现的，会对传入的key的自然排序或者实现了Comparator比较器进行排序
- 时间复杂度为log(n)

### put方法
```
如果没有初始化就先调用initTable（）方法来进行初始化过程

如果没有hash冲突就直接CAS插入

如果还在进行扩容操作就先进行扩容

如果存在hash冲突，就加锁来保证线程安全，这里有两种情况，一种是链表形式就直接遍历到尾端插
入，一种是红黑树就按照红黑树结构插入，最后一个如果该链表的数量大于阈值8，就要先转换成黑红树的结构，break再一次进入循环

如果添加成功就调用addCount（）方法统计size，并且检查是否需要扩容
```


### 如何用treeMap实现一致性Hash算法
```java
class Shard<S> { // S类封装了机器节点的信息 ，如name、password、ip、port等
 
    private TreeMap<Long, S> nodes; // 虚拟节点
    private List<S> shards; // 真实机器节点
    private final int NODE_NUM = 100; // 每个机器节点关联的虚拟节点个数
 
    public Shard(List<S> shards) {
        super();
        this.shards = shards;
        init();
    }
 
    private void init() { // 初始化一致性hash环
        nodes = new TreeMap<Long, S>();
        for (int i = 0; i != shards.size(); ++i) { // 每个真实机器节点都需要关联虚拟节点
            final S shardInfo = shards.get(i);
 
            for (int n = 0; n < NODE_NUM; n++)
                // 一个真实机器节点关联NODE_NUM个虚拟节点
                nodes.put(hash("SHARD-" + i + "-NODE-" + n), shardInfo);
 
        }
    }
 
    public S getShardInfo(String key) {
        SortedMap<Long, S> tail = nodes.tailMap(hash(key)); // 沿环的顺时针找到一个虚拟节点
        if (tail.size() == 0) {
            return nodes.get(nodes.firstKey());
        }
        return tail.get(tail.firstKey()); // 返回该虚拟节点对应的真实机器节点的信息
    }
 
    /**
     *  MurMurHash算法，是非加密HASH算法，性能很高，
     *  比传统的CRC32,MD5，SHA-1（这两个算法都是加密HASH算法，复杂度本身就很高，带来的性能上的损害也不可避免）
     *  等HASH算法要快很多，而且据说这个算法的碰撞率很低.
     *  http://murmurhash.googlepages.com/
     */
    private Long hash(String key) {
 
        ByteBuffer buf = ByteBuffer.wrap(key.getBytes());
        int seed = 0x1234ABCD;
 
        ByteOrder byteOrder = buf.order();
        buf.order(ByteOrder.LITTLE_ENDIAN);
 
        long m = 0xc6a4a7935bd1e995L;
        int r = 47;
 
        long h = seed ^ (buf.remaining() * m);
 
        long k;
        while (buf.remaining() >= 8) {
            k = buf.getLong();
 
            k *= m;
            k ^= k >>> r;
            k *= m;
 
            h ^= k;
            h *= m;
        }
 
        if (buf.remaining() > 0) {
            ByteBuffer finish = ByteBuffer.allocate(8).order(
                    ByteOrder.LITTLE_ENDIAN);
            // for big-endian version, do this first:
            // finish.position(8-buf.remaining());
            finish.put(buf).rewind();
            h ^= finish.getLong();
            h *= m;
        }
 
        h ^= h >>> r;
        h *= m;
        h ^= h >>> r;
 
        buf.order(byteOrder);
        return h;
    }
 
}

```