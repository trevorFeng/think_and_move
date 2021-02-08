### 广度优先搜索
算法框架
```
int bfs(Node start, Node target){
    Queue<Node> queue = new Queue();
    Set<Node> set = new HashSet();//避免回头路
    Node p = start;
    queue.offer(p);
    set.add(p);
    int step = 0;//记录步数
    while(queue.isNotEmpty()) {
        int size = queue.size();
        //将当前队列向四周扩散
        for(int i=0;i<size;i++){
            Node cur = queue.poll();
            if(cur is target) {
                return strp;
            }
            //将相邻节点加入队列
            for (Node temp : cur.adj()){
                if(!set.contains(temp)) {
                    queue.offer(temp);
                }
            }        
        }
        step++; 
    }
    
}

```