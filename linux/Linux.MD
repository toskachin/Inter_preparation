## Questions

* Linux performance tools

![image](images/Linux_performance_tools.png)

* How to show ip address only on Centos
  
`hostname -i`

* explain uptime means

```
 15:31:30 up 127 days,  3:00,  1 user,  load average: 0.00, 0.00, 0.0
15:31:30             # 系统当前时间
up 127 days,  3:00   # 主机已运行时间,时间越大，说明你的机器越稳定。
1 user               # 用户连接数，是总连接数而不是用户数
load average: 0.00, 0.00, 0.00         #  系统平均负载，统计最近1，5，15分钟的系统平均负载
```

* 计算文件多少空行

```
$ grep -c  "^$" messages anaconda.log
messages:0
anaconda.log:3
```

* List high memory process in Linux

```
ps -eo pid,ppid,%mem,%cpu,cmd --sort=-%mem | grep -v '0.0  0.0' | cut -c-$(($(tput cols) - 5)); echo ' '; free -g; date
```

* Linux 系统性能命令
  
cpu:
- cpu 7 中状态
	- cpu 空闲
	- cpu 可以运行用户代码，指的是“用户”时间
	- 
- interrupts - 上下文切换
	- cat /proc/interrupts | grep timer
- 命令
	- vmstat [ -n ] [ -s ] 
		- -n
		- -s
	- top
		- f/F - 显示一个配置界面，可以配置选项
		- P
	- procinfo
	- mpstat
		- -P  告诉mpstat 监控那个cpu 
	- sar
		- -W
		- -P  监控哪个cpu
	- oprofile
		- -r
- cpu 性能分析
	- gprof
	- strace
	- ps 
		- ps -o etim,time,pcpu,cmd  1008(pid)
	- ltrace
	- 

memory
- 概念
	- 交换 -swap 
	- 缓冲区 buffer,缓存区cache
	- 活跃和非活跃内存
	- 高端和低端内存
	- 内核的内存使用情况（分片）
- tools
	- vmstat
		- -m
		- -a
		- -s
	- top
		- M
	- proinfo
	- free 
		- -m
		- -h
	- sar 
		- -B
		- -W
		- -r
	- cat /proc/meminfo

CPU
- top
	- 按数字1 显示每个cpu  usage
	- top -Hp pid 
- sar -u 
- vmstat
	- 只能看整体cpu情况
```
r 表示运行队列(就是说多少个进程真的分配到CPU)
b 表示阻塞的进程,这个不多说，进程阻塞
si  每秒从磁盘读入虚拟内存的大小，如果这个值大于0，表示物理内存不够用或者内存泄露了，要查找耗内存进程解决掉。我的机器内存充裕，一切正常
so  每秒虚拟内存写入磁盘的大小，如果这个值大于0，同上。
bi  块设备每秒接收的块数量，这里的块设备是指系统上所有的磁盘和其他块设备，默认块大小是1024byte，我本机上没什么IO操作，所以一直是0，但是我曾在处理拷贝大量数据(2-3T)的机器上看过可以达到140000/s，磁盘写入速度差不多140M每秒
bo 块设备每秒发送的块数量，例如我们读取文件，bo就要大于0。bi和bo一般都要接近0，不然就是IO过于频繁，需要调整。
in 每秒CPU的中断次数，包括时间中断
cs 每秒上下文切换次数，例如我们调用系统函数，就要进行上下文切换，线程的切换，也要进程上下文切换，这个值要越小越好，太大了，要考虑调低线程或者进程的数目,例如在apache和nginx这种web服务器中，我们一般做性能测试时会进行几千并发甚至几万并发的测试，选择web服务器的进程可以由进程或者线程的峰值一直下调，压测，直到cs到一个比较小的值，这个进程和线程数就是比较合适的值了。系统调用也是，每次调用系统函数，我们的代码就会进入内核空间，导致上下文切换，这个是很耗资源，也要尽量避免频繁调用系统函数。上下文切换次数过多表示你的CPU大部分浪费在上下文切换，导致CPU干正经事的时间少了，CPU没有充分利用，是不可取的。
us 用户CPU时间，我曾经在一个做加密解密很频繁的服务器上，可以看到us接近100,r运行队列达到80(机器在做压力测试，性能表现不佳)。
sy 系统CPU时间，如果太高，表示系统调用时间长，例如是IO操作频繁。
id  空闲 CPU时间，一般来说，id + us + sy = 100,一般我认为id是空闲CPU使用率，us是用户CPU使用率，sy是系统CPU使用率。
wt 等待IO CPU时间
```
- mpstat
	- 查看cpu计算核心数据
	- Multiprocessor Statistics
	- /proc/stat
```
%user      在internal时间段里，用户态的CPU时间(%)，不包含nice值为负进程  (usr/total)*100
%nice      在internal时间段里，nice值为负进程的CPU时间(%)   (nice/total)*100
%sys       在internal时间段里，内核时间(%)       (system/total)*100
%iowait    在internal时间段里，硬盘IO等待时间(%) (iowait/total)*100
%irq       在internal时间段里，硬中断时间(%)     (irq/total)*100
%soft      在internal时间段里，软中断时间(%)     (softirq/total)*100
%idle      在internal时间段里，CPU除去等待磁盘IO操作外的因为任何原因而空闲的时间闲置时间(%) (idle/total)*100
```

- dmesg 
Memory
- free -mh
- sar -r 2 4
- sar -B 
	- page statistics report
```
In the report majflts/s shows the major faults per second means number of pages loaded into the memory from disk (swap), if its value is higher then we can say that system is running of RAM.

%vmeff indicates the number of pages scanned per second, if it’s vaule is 100 % its is consider OK and when it is below 30 % then there is some issue with virtual memory. Zero value indicates that there is no page scanned during that time.
```
Disk/Storage
- iostat
```
-C 显示CPU使用情况
-d 显示磁盘使用情况
-k 以 KB 为单位显示
-m 以 M 为单位显示
-N 显示磁盘阵列(LVM) 信息
-n 显示NFS 使用情况
-p[磁盘] 显示磁盘和分区的情况
```
	- iostat -d -k 2 2
```
tps：该设备每秒的传输次数（Indicate the number of transfers per second that were issued to the device.）。“一次传输”意思是“一次I/O请求”。多个逻辑请求可能会被合并为“一次I/O请求”。“一次传输”请求的大小是未知的。
kB_read/s：每秒从设备（drive expressed）读取的数据量；
kB_wrtn/s：每秒向设备（drive expressed）写入的数据量；
kB_read：读取的总数据量；kB_wrtn：写入的总数量数据量；
这些单位都为Kilobytes。
上面的例子中，我们可以看到磁盘sda以及它的各个分区的统计数据，当时统计的磁盘总TPS是22.73，下面是各个分区的TPS。（因为是瞬间值，所以总TPS并不严格等于各个分区TPS的总和）
```
	- iostat -d -x -k 2 2
```
rrqm/s：  每秒进行 merge 的读操作数目.即 delta(rmerge)/s
wrqm/s： 每秒进行 merge 的写操作数目.即 delta(wmerge)/s
r/s：  每秒完成的读 I/O 设备次数.即 delta(rio)/s
w/s：  每秒完成的写 I/O 设备次数.即 delta(wio)/s
rsec/s：  每秒读扇区数.即 delta(rsect)/s
wsec/s： 每秒写扇区数.即 delta(wsect)/s
rkB/s：  每秒读K字节数.是 rsect/s 的一半,因为每扇区大小为512字节.(需要计算)
wkB/s：  每秒写K字节数.是 wsect/s 的一半.(需要计算)
avgrq-sz：平均每次设备I/O操作的数据大小 (扇区).delta(rsect+wsect)/delta(rio+wio)
avgqu-sz：平均I/O队列长度.即 delta(aveq)/s/1000 (因为aveq的单位为毫秒).
await：  平均每次设备I/O操作的等待时间 (毫秒).即 delta(ruse+wuse)/delta(rio+wio)
svctm： 平均每次设备I/O操作的服务时间 (毫秒).即 delta(use)/delta(rio+wio)
%util： 一秒中有百分之多少的时间用于 I/O 操作,或者说一秒中有多少时间 I/O 队列是非空的，即 delta(use)/s/1000 (因为use的单位为毫秒)
如果 %util 接近 100%，说明产生的I/O请求太多，I/O系统已经满负荷，该磁盘可能存在瓶颈。
idle小于70% IO压力就较大了，一般读取速度有较多的wait。
同时可以结合vmstat 查看查看b参数(等待资源的进程数)和wa参数(IO等待所占用的CPU时间的百分比，高过30%时IO压力高)。
另外 await 的参数也要多和 svctm 来参考。差的过高就一定有 IO 的问题。
avgqu-sz 也是个做 IO 调优时需要注意的地方，这个就是直接每次操作的数据的大小，如果次数多，但数据拿的小的话，其实 IO 也会很小。如果数据拿的大，才IO 的数据会高。也可以通过 avgqu-sz × ( r/s or w/s ) = rsec/s or wsec/s。也就是讲，读定速度是这个来决定的。
svctm 一般要小于 await (因为同时等待的请求的等待时间被重复计算了)，svctm 的大小一般和磁盘性能有关，CPU/内存的负荷也会对其有影响，请求过多也会间接导致 svctm 的增加。await 的大小一般取决于服务时间(svctm) 以及 I/O 队列的长度和 I/O 请求的发出模式。如果 svctm 比较接近 await，说明 I/O 几乎没有等待时间；如果 await 远大于 svctm，说明 I/O 队列太长，应用得到的响应时间变慢，如果响应时间超过了用户可以容许的范围，这时可以考虑更换更快的磁盘，调整内核 elevator 算法，优化应用，或者升级 CPU。
队列长度(avgqu-sz)也可作为衡量系统 I/O 负荷的指标，但由于 avgqu-sz 是按照单位时间的平均值，所以不能反映瞬间的 I/O 洪
```
- sar 2 5 
```
If the %I/O wait is more than zero for a longer period of time then we can consider there is some bottleneck in I/O system ( Hard disk or Network )
```
- sar -d 

Jobs/process
- jobs -l
- bg
- nice/renice
- ps aux/-ef | grep xxx
- pidstat
- lsof
Network
- ss -nltp | grep xxx
- netstat -ntpl  | grep xxx
- ip route
- iptables

Iostat
iotop
pidstat
vmstat
lsof
ss
free -mh
uname -a

Files manage
- /proc   - process information
- /var  -variable data, most notably log files
- /usr - user related programs
- /home  - home directories
- /etc - system configuration files
- opt  - optional or three party software
- /dev - device files
- /bin - Binaries and other executable programs.
- /sbin  -similar to /bin with the exception that these ready to run binaries are reserved for root users
- /srv  - Srv is a serve folder. It holds site specific data to be served by the system for protocols such as, ftp, rsync, www, cvs etc
- /lib - Lib contains system library binaries that are required to run the system
- /sys - This is kinda like /proc it is used for plug and play configuration
- /root - Root is the “home” directory for root.
- /mnt - This directory is provided so that the system administrator may temporarily mount a filesystem as needed



## Docker commands

docker system prune [OPTIONS]

docker build --no-cache -f Dockerfile -t ci-centos6-base .

docker exec -it containerID /bin/bash 

docker run -it xxxxxxx:5000/ubuntu-mysqldb-mvn /bin/bash

docker image rm -f ci-python36-test

docker tag 98d6830c1011 lxxxxxt:5000/python-xxx


curl -s -k -X GET https://lxxxxxx:4000/v2/_catalog | jq

 docker update --cpu-shares 512 abebf7571666![Alt text](image.png)