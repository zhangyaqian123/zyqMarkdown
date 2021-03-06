# JVM虚拟机

# 1. JVM与java体系结构

## 1.7 java代码执行流程

java源码(xxx.java) —→ java编译器 —→ 字节码(xxx.class) → java虚拟机（类加载器 → 字节码校验器 → 翻译字节码(执行引擎完成)） → 生成机器指令 → 操作系统

![Untitled](Untitled%201.assets/Untitled.png)

## 1.8 jvm的架构模型

指令集架构可分为两种：基于栈的指令集架构；基于寄存器的指令集架构

（HotSpot）java编译器输入的指令流属于基于栈的指令集架构

区别：

- 栈
  - 设计和实现更加简单，适合资源受限制的系统；（只用出栈入栈）
  - 使用零地址指令方式分配，指令集更小，编译器容易实现
  - 不需要硬件支持（内存级别），可移植性好，更好实现跨平台
- 寄存器
  - 典型应用：X86的二进制指令集：如传统的PC和Android的Davlik虚拟机
  - 完全依赖硬件，可移植性差
  - *性能优秀且执行更搞高效（指令在CPU中执行，速度快）*
  - 花费的指令更少（将java代码的字节码反编译，栈的指令会更多）
  - 指令集以一地址指令、二地址指令、三地址指令为主：在做一个指令的运算时，一般需要两部分，指令地址和操作数，一地址指令指其地址是一位的，零地址指令指没有地址只有操作数（栈不需要地址，只用对栈顶元素操作）

### 总结：

由于跨平台性，java的指令都是通过栈来设计的，因为不同平台的CPU架构不一样。优点是跨平台，指令集小，编译器容易实现；缺点是性能下降，实现同样的功能需要更多指令

## 1.9 JVM的生命周期

1. 虚拟机的启动

   官网：java虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类来完成的，这个类是由虚拟机的具体实现指定的。

2. 执行

   - 一个运行中的java虚拟机有一个清晰的任务：执行java程序
   - 程序开始执行时它运行，程序结束时它停止
   - 执行一个所谓的java程序时，真正在执行的是一个叫做java虚拟机的进程

3. 退出

# 2. 类加载子系统

## 2.1 内存结构概述

简图：

![1](Untitled%201.assets/1-1614603492489-1614603921632.png)

详细图：

![2](Untitled%201.assets/2-1614603925666.png)

## 2.2 类加载器和类的加载过程

### 2.2.1 类加载子系统作用

类加载器主要是将字节码文件加载到内存中，生成Class示例。

- 类加载子系统负责从文件系统或网络中加载Class文件，*class文件在开头有特殊的标志* .
- ClassLoasder只负责class文件的加载，至于它是都可以运行，则由执行引擎决定
- 加载的类信息存放在：方法区。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包括字符串字面量和数字常量

### 2.2.2 类加载器ClassLoader角色

![3](Untitled%201.assets/3-1614603927436.png)

1. class文件存在于本地磁盘中，在执行的时候需要加载到JVM中，根据这个文件实例化出n个一模一样的示例。
2. *class文件加载到jvm中，被称为DHA元数据模板，存放在方法区*
3. 在.class文件 -》 JVM  -》最终成为元数据模板，这个过程需要一个运输工具，Class Loader 扮演这个快递员的角色。

### 2.2.3 类加载过程

大致可以分为：

加载 ——》连接 ——》初始化

1. 加载：

   - 通过类的全限定名称获取定义此类的二进制字节流（例如从物理磁盘上获取文件）

   - 将这个字节流所代表的静态储存结构，转化为方法区的运行时数据结构（类信息保存在方法区）

   - 在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口。

     **补充：加载 .class文件的方式**

     - 本地文件
     - 网络获取：Web Applet
     - 从zip压缩包：jar,war格式
     - 运行时计算：动态代理技术
     - 。。。。

2. 连接：

   1. 验证 Verify
      - 目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全。
      - 主要包括四种验证，文件格式验证，源数据验证，字节码验证，符号引用验证。
   2. 准备  Prepare
      - 为类变量分配内存并且设置该类变量的*默认初始值，即**零值***；
      - 这里不包含用final修饰的static，因为final在编译的时候就会分配了，准备阶段会显式初始化；
      - *这里不会为实例变量分配初始化，类变量会分配在方法去中，而实例变量是会随着对象一起分配到java堆中*。

   3. 解析 Resolve

   - 将常量池内的符号引用转换为直接引用的过程。（常量池前#31）
   - 事实上，解析操作往往会伴随着jvm在执行完初始化之后再执行
   - 符号引用就是一组符号来描述所引用的目标。符号应用的字面量形式明确定义在《java虚拟机规范》的class文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄
   - 解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的CONSTANT_Class_info/CONSTANT_Fieldref_info、CONSTANT_Methodref_info等。