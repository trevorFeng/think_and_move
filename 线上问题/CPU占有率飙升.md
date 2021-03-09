### 使用top命令
```shell script
top
PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
```
找到cpu占有率高的java进程

### 定位线程
使用下面命令

```
$top -Hp1893

PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+       COMMAND
4519 admin    20  0   7127  m2.6g38mR18.63          2.60:40.11  java
```

查询PID为1893的这个java进程各个线程的CPU使用情况


### 定位代码
将线程的PID转换为16进制

printf %x\n 4519

11a7


使用jstack命令，查看栈信息
sudo -u admin  jstack1893|grep -A20011a7

