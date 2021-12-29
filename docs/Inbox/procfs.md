伪文件系统/proc，它是内核中各属性或状态向外提供访问和修改的接口。在/proc下，记录了内核自己的数据信息，各进程独立的数据信息，统计信息等。绝大多数文件都是只读不可改的，即使对root也一样，但/proc/sys除外，

非数字命令各有用途

echo b > /proc/sysrq-trigger  # 硬重启节点

| 路径         |                             |                                    |
| ------------ | --------------------------- | ---------------------------------- |
| /proc/self   | 当前正在访问/proc目录的进程 | cat /proc/self/cmdline             |
| /proc/sys    | 修改内核参数，roo可写       | sysctl 本质修改/proc/sys  中的文件 |
| /proc/net    |                             |                                    |
| /proc/tty    |                             |                                    |
| /proc/bus    |                             |                                    |
| /proc/driver |                             |                                    |
| /proc/fs     |                             |                                    |

所谓负载率(load)，即特定时间长度内，cpu运行队列中的平均进程数(包括线程)，一般平均每分钟每核的进程数小于3都认为正常，大于5时负载已经非常高。在UNIX系统中，运行队列包括cpu正在执行的进程和等待cpu的进程(即所谓的可运行runable)。在Linux系统中，还包括不可中断睡眠态(IO等待)的进程。运行队列中每出现一个进程，load就加1，进程每退出运行队列，Load就减1。如果是多核cpu，则还要除以核数。

- 获取正在运行的队列(R), 不可中断睡眠(D)	

    ```shell
    ps -eo stat,pid,ppid,comm --no-header |grep -E "^(D|R)"
    ```

- 清除内存buffer cache

  ```shell
  sync
  sync
  sync
  
  echo 1 > /proc/sys/vm/drop_caches # 清除page cache
  echo 2 > /proc/sys/vm/drop_caches # 清除slab分配器中的对象
  echo 3 > /proc/sys/vm/drop_caches # 清除page cache和slab分配器中的缓存对象
  ```

- vmstat

    ```shell
    Procs
       r: 等待队列中的进程数
       b: 不可中断睡眠的进程数
    Memory
    
       swpd: 虚拟内存使用总量
       free: 空闲内存量
       buff: buffer占用的内存量(buffer用于缓冲)
       cache: cache占用的内存量(cache用于缓存)
    Swap
    
       si:从磁盘加载到swap分区的数据流量，单位为"kb/s"
       so: 从swap分区写到磁盘的数据流量，单位为"kb/s"
    IO
    
       bi: 从块设备接受到数据的速率，单位为blocks/s
       bo: 发送数据到块设备的速率，单位为blocks/s
    System
    
       in: 每秒中断数，包括时钟中断数量
       cs: 每秒上下文切换次数
    CPU：统计的是cpu时间百分比，具体信息和top的cpu统计列一样
    
       us: Time spent running non-kernel code. (user time, including nice time)
       sy: Time spent running kernel code. (system time)
       id: Time spent idle. Prior to Linux 2.5.41, this includes IO-wait time.
       wa: Time spent waiting for IO. Prior to Linux 2.5.41, included in idle.
       st: Time stolen from a virtual machine. Prior to Linux 2.6.11, unknown.
    ```

    

