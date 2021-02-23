### binLog与redo日志的区别
- redo log是InnoDB引擎特有的；binlog是MySQL的Server层实现的，所有引擎都可以使用
- redo log是物理日志，记录的是“在XXX数据页上做了XXX修改”；binlog是逻辑日志，记录的是原始逻辑，其记录是对应的SQL语句
- redo log是循环写的，空间一定会用完，需要write pos和check point搭配；binlog是追加写，写到一定大小会切换到下一个，并不会覆盖以前的日志

### 写入时机
事务最终commit前写入的

### 作用
- MySQL的备份恢复
- MySQL的主从复制
- 用来查看MySQL变更

### 三种模式
- STATEMENT  
仅仅记录变更的sql，不会记录数据行的变化，I/O次数少，性能高，在某些情况下会导致master-slave中的数据不一致（在主库和从库执行sql使用的索引不一样）

- ROW  
记录行变更之前和变更之后的数据，日志量大

- MINED
使用上面两个