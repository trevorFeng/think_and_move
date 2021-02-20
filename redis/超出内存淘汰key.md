- volatile-lru 设置了过期时间的key参与近似的lru淘汰策略
- allkeys-lru 所有的key均参与近似的lru淘汰策略

随机抽取出5个key，淘汰最旧的，依次循环，知道内存不再超出限制