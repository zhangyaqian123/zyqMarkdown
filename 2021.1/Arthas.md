# Arthas使用

## 1. 是什么&解决什么问题&适用场景

​	Arthas是Alibaba开源的java诊断工具，安装在系统所在服务器，可以帮助开发人员查找问题，分析性能，bug追踪。

----

### 1.1 解决的问题

​	1、以全局视角来查看系统的运行状况、健康状况。
​	2、反编译源码，查看jvm加载的是否为预期的文件内容。
​	3、***查看某个方法的返回值，参数等等。***
​	4、方法内调用路径及各方法调用耗时。
​	5、***查看jvm运行状况。***
​	6、外部.class文件重新加载到jvm里。

### 1.2 场景

1）调用接口时，接口返回异常信息，如果该*异常信息没有清晰的定位到代码*，那么我们通常只能依靠大脑回忆代码，来估计错误发生地了，如果无法估计，一般情况下就会进入测试环境，模拟复现，如果无法复现 _。
2）这个查询，*耗时20s，我们想要分析一下到底是哪些代码导致的*。但是该方法内部又穿插调用了其它业务功能方法，难道手写System.currentTimeMillis()自己做减运算，还是guava的StopWatch亦或是commons的StopWatch？这几种方式需要我们手动嵌入代码，容易遗漏、费力还费时。

-------

## 2. 常用指令

```java
基础命令
help——查看命令帮助信息
cls——清空当前屏幕区域
session——查看当前会话的信息
reset——重置增强类，将被 Arthas 增强过的类全部还原，Arthas 服务端关闭时会重置所有增强过的类
version——输出当前目标 Java 进程所加载的 Arthas 版本号
quit——退出当前 Arthas 客户端，其他 Arthas 客户端不受影响
shutdown——关闭 Arthas 服务端，所有 Arthas 客户端全部退出
keymap——Arthas快捷键列表及自定义快捷键

jvm相关
dashboard——当前系统的实时数据面板
thread——查看当前 JVM 的线程堆栈信息(非常有用的一个命令)
jvm——查看当前 JVM 的信息
sysprop——查看和修改JVM的系统属性
getstatic——查看类的静态属性
class/classloader相关
sc——查看JVM已加载的类信息(可以查看类是哪个jar包中加载的)
sm——查看已加载类的方法信息
dump——dump 已加载类的 byte code 到特定目录
redefine——加载外部的.class文件，redefine到JVM里
jad——反编译指定已加载类的源码
classloader——查看classloader的继承树，urls，类加载信息，使用classloader去getResource
monitor/watch/trace相关
请注意，这些命令，都通过字节码增强技术来实现的，会在指定类的方法中插入一些切面来实现数据统计和观测，因此在线上、预发使用时，请尽量明确需要观测的类、方法以及条件， 诊断结束要执行 shutdown 或将增强过的类执行 reset 命令。
    
monitor——方法执行监控

watch——方法执行数据观测

trace——方法内部调用路径，并输出方法路径上的每个节点上耗时

stack——输出当前方法被调用的调用路径

tt——方法执行数据的时空隧道，记录下指定方法每次调用的入参和返回信息，并能对这些不同的时间下调用进行观测

options
options——查看或设置Arthas全局开关
```

-----

## 3. 安装和启动

```
下载在随意盘中：https://arthas.aliyun.com/doc/install-detail.html#id2
在当前文件夹-cmd：java -jar arthas-boot.jar
```

根据提示选择想要监控的进程。

![image-20210106110925962](C:\Users\zhangyq-i\AppData\Roaming\Typora\typora-user-images\image-20210106110925962.png)

## 4. 相关命令参数含义简析

- **dashbord：** 显示当前进程相关信息

```java
____________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
|  ①Thread相关信息                                                                                                                                                    
|  线程id              线程名称                                                      线程组                                  线程优先级            线程状态             线程消耗的cpu百分比   运行总时间           线程当前的中断位状态    是否守护线程
|  ID                  NAME                                                        GROUP                                   PRIORITY            STATE               %CPU                TIME                INTERRUPTED         DAEMON
|  188                 Timer-for-arthas-dashboard-f5864b5b-762a-4fb5-8cc5-65559bd6 system                                  10                  RUNNABLE            19                  0:0                 false               true
|  36                  pool-1-thread-1                                             main                                    5                   TIMED_WAITING       5                   0:1                 false               false
|  33                  Abandoned connection cleanup thread                         main                                    5                   TIMED_WAITING       0                   0:0                 false               true
|  179                 AsyncAppender-Worker-arthas-cache.result.AsyncAppender      system                                  9                   WAITING             0                   0:0                 false               true
|  12                  AsyncFileHandlerWriter-225534817                            main                                    5                   TIMED_WAITING       0                   0:0                 false               true
|  94                  Attach Listener                                             system                                  9                   RUNNABLE            0                   0:0                 false               true
|  70                  ContainerBackgroundProcessor[StandardEngine[Catalina]]      main                                    5                   TIMED_WAITING       0                   0:0                 false               true
|  34                  Druid-ConnectionPool-Create-300669762                       main                                    5                   WAITING             0                   0:0                 false               true
|  35                  Druid-ConnectionPool-Destroy-300669762                      main                                    5                   TIMED_WAITING       0                   0:0                 false               true
|  3                   Finalizer                                                   system                                  8                   WAITING             0                   0:0                 false               true
|  13                  GC Daemon                                                   system                                  2                   TIMED_WAITING       0                   0:0                 false               true
|  14                  NioBlockingSelector.BlockPoller-1                           main                                    5                   RUNNABLE            0                   0:0                 false               true
|  15                  NioBlockingSelector.BlockPoller-2                           main                                    5                   RUNNABLE            0                   0:0                 false               true
|  2                   Reference Handler                                           system                                  10                  WAITING             0                   0:0                 false               true
|  4                   Signal Dispatcher                                           system                                  9                   RUNNABLE            0                   0:0                 false               true
|  76                  ajp-nio-38009-Acceptor-0                                    main                                    5                   RUNNABLE            0                   0:0                 false               true
|  74                  ajp-nio-38009-ClientPoller-0                                main                                    5                   RUNNABLE            0                   0:0                 false               true
|  75                  ajp-nio-38009-ClientPoller-1                                main                                    5                   RUNNABLE            0                   0:0                 false               true
|  187                 as-command-execute-daemon                                   system                                  10                  TIMED_WAITING       0                   0:0                 false               true
|  73                  http-nio-37080-Acceptor-0                                   main                                    5                   RUNNABLE            0                   0:0                 false               true
|  71                  http-nio-37080-ClientPoller-0                               main                                    5                   RUNNABLE            0                   0:0                 false               true
|  72                  http-nio-37080-ClientPoller-1                               main                                    5                   RUNNABLE            0                   0:0                 false               true
|
|  ②内存信息                                                                                                               ③垃圾回收
|  Memory                                             used              total            max              usage            GC
|  （堆）                                                                                                                   （垃圾回收次数）
|  heap                                               424M              1897M            1897M            22.37%           gc.ps_scavenge.count                                  19
|  （伊甸园）                                                                                                                （垃圾回收消耗时间）
|  ps_eden_space                                      311M              387M             403M             77.28%           gc.ps_scavenge.time(ms)                               1405
|  （幸存者区）                                                                                                              （标记-清除算法的次数）
|  ps_survivor_space                                  40M               144M             144M             27.74%           gc.ps_marksweep.count                                 3
|  （老年代）                                                                                                               （标记-清除算法的消耗时间）
|  ps_old_gen                                         72M               1365M            1365M            5.32%            gc.ps_marksweep.time(ms)                              446
|  （非堆区）                                                                                                                                                                                                                                                                                                                                                                        
|  nonheap                                            137M              141M             -1               97.49%                                                                                                                                   
|  （代码缓存区）                                                                                                                                                                                                                                                                                                                                                                   
|  code_cache                                         40M               41M              240M             16.99%                                                                                                                     
|  （元空间）                                                                                                                                                                                    
|  metaspace                                          86M               89M              -1               97.09%                                                                                                                                   
|  （压缩空间）                                                                                                                                                                                    
|  compressed_class_space                             10M               10M              1024M            0.99%                                                                                                                                    
|  direct                                             80K               80K              -                100.00%                                                                                                                                  
|  mapped                                             0K                0K               -                NaN%                                                                                                                                     
|                                                                                                                                                                                      
|  ④运行信息                                                                                                                                                                                    
|  Runtime                                                                                                                                                                                                                                                                                                                                                                        
|  os.name                                                                                                                 Linux                                                                                                                   
|  os.version                                                                                                              3.10.0-957.1.3.el7.x86_64                                                                                               
|  java.version                                                                                                            1.8.0_101                                                                                                               
|  java.home                                                                                                               /opt/jdk1.8.0_101/jre                                                                                                   
|  systemload.average                                                                                                      0.03                                                                                                                    
|  processors                                                                                                              8                                                                                                                       
|  uptime                                                                                                                  11956s                                                                                                                  
|________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

说明
ID: Java级别的线程ID，注意这个ID不能跟jstack中的nativeID一一对应
NAME: 线程名
GROUP: 线程组名
PRIORITY: 线程优先级, 1~10之间的数字，越大表示优先级越高
STATE: 线程的状态
CPU%: 线程消耗的cpu占比，采样100ms，将所有线程在这100ms内的cpu使用量求和，再算出每个线程的cpu使用占比。
TIME: 线程运行总时间，数据格式为分：秒
INTERRUPTED: 线程当前的中断位状态
DAEMON: 是否是daemon线程

通过上述信息，可以帮助我们快速定位相关问题线程。

```

- 查看具体线程信息使用[thread 线程id]



- **watch**:查看类中某方法的返回值和入参--watch

```
命令+类完全限定名+监测方法+表达式
watch cn.asae.e.contract.web.ContractSubjectController getContractSubjectLogs “{params,returnObj} -s -n 300”
```

（1） 方法返回后 打印返回值

```
watch cn.asae.e.contract.web.ContractSubjectController getContractSubjectLogs "{returnObj}" -s -n 1

```

（2）方法执行前 打印参数

```
watch kite.springcloud.jxm.service.MonitorDashboardService buildJvmInfo "{params}" -b -n 1

```

（3）方法返回后 打印所在的Classloard、类、调用的实例

```
watch kite.springcloud.jxm.service.MonitorDashboardService buildJvmInfo "{loader,clazz,target}" -s -n 1
```



```
表达式核心变量列表：
loader      本次调用类所在的 ClassLoader
clazz       本次调用类的 Class 引用
method      本次调用方法反射引用
target      本次调用类的实例
params      本次调用参数列表，这是一个数组，如果方法是无参方法则为空数组
returnObj   本次调用返回的对象。当且仅当 isReturn==true 成立时候有效，表明方法调用是以正常返回的方式结束。如果当前方法无返回值 void，则值为 null
watch 命令定义了4个观察事件点，即 -b 方法调用前，-e 方法异常后，-s 方法返回后，-f 方法结束后
4个观察事件点 -b、-e、-s 默认关闭，-f 默认打开，当指定观察点被打开后，在相应事件点会对观察表达式进行求值并输出
-n  次数，达到后监控停止
```

- **trace:** 方法内部调用路径，并输出方法路径上的每个节点耗时

  -j 过滤掉jdk方法

```
trace cn.asae.e.contract.web.ContractController getContract -j
```

