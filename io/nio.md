### java多路复用的实现

### 三个概念
- channel
- 缓冲区
- Selector选择器

### 原理
channel向selector注册，selector能够检测多个注册的通道上是否有事件发生，然后便针对每个事件做出响应，这样单线程就够处理多个连接