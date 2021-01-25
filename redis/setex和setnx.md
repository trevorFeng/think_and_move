### setex
```
setex key seconds value
```
- 原子操作
- 指定 key 的值为 value，并将 key 的过期时间设为 seconds (以秒为单位),若key存在，则覆盖

### setnx
```
setnx key value
```
- 只有key在不存在时设置key的值