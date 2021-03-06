### 1.8对1.7的优化
- 而当链表长度太长（TREEIFY_THRESHOLD默认超过8）时，链表就转换为红黑树，利用红黑树快速增删改查的特点提高HashMap的性能（O(logn)）。当长度小于（UNTREEIFY_THRESHOLD默认为6），就会退化成链表

- 扩容的时候采用了尾插法

- 由于容量扩大两倍，hash的二进制数就是在高位加一，所以重新计算key的hash值的时候高位不是0就是1，所以重新计算的hash值就是原来的索引+原来数组的容量或者保持不变

### 1.7的成环问题
- 假设A，B线程，map在同一个位置有两个元素1->2
- A线程跑到Entry<K,V> next = e.next()时挂起，
- B线程执行完方法，扩容后2->1
- A线程继续执行，第一次循环e=1,next=2,新的table[i]=1;
- 第二次循环，e=2,由于B线程改了引用，导致next=1,此时新的table[i]=2,2->1
- 第三次循环，e=1,next=null,此时新的table[i]=1,1->2,2->1,导致get()死循环

### 扩容的时机
- 先put再扩容
- 当map中的数量大于加载因子 * 数组容量的扩容

### hashmap的数组长度为什么要保证是2的幕
- 数组的容量n是2的2次幂
- key放在数组的哪个位置有（n-1） & hash（等价于取模，但效率更高）公式计算得出（&是两个1才是1）
- 所以n-1后面全是1，在与的时候会保留hash中后x位的1
- 能保证索引小于容量