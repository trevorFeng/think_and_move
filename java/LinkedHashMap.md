### LinkedHashMap
将Entry<K,V>节点链入双向链表的HashMap，额外维护了一个双向链表用于保持迭代顺序

### 构造器
```
    public LinkedHashMap() {
        super();
        //为false表示基于插入顺序排序，true为基于访问顺序排序
        accessOrder = false;
    }
```

### 若accessOrder为false
```
map.put(2,2);
map.put(3,4);
map.put(4,4);
```
则遍历的时候key值依次为2，3，4

### 若accessOrder为true
```
map.put(2,2);
map.put(3,4);
map.put(4,4);
```
先get(2),则遍历的时候key值依次为3，4，2

说明访问了一次，将key移到了队尾


### 如何用 LinkedHashMap 实现 LRU
- 实现一个类继承LinkedHashMap
- 定义容量，accessOrder为true
- 重写removeEldestEntry方法
```java
class LRULinkedHashMap<K,V> extends LinkedHashMap<K,V>{
	//定义缓存的容量
	private int capacity;
	private static final long serialVersionUID = 1L;
	//带参数的构造器	
	LRULinkedHashMap(int capacity){
		//调用LinkedHashMap的构造器，传入以下参数
		super(16,0.75f,true);
		//传入指定的缓存最大容量
		this.capacity=capacity;
	}
	//实现LRU的关键方法，如果map里面的元素个数大于了缓存最大容量，则删除链表的顶端元素
	@Override
	public boolean removeEldestEntry(Map.Entry<K, V> eldest){ 
		System.out.println(eldest.getKey() + "=" + eldest.getValue());  
		return size()>capacity;
	}  
}
```