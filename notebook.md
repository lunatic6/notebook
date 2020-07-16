# 1. Java基础知识

## 1. 集合

### 1. HashMap

- 默认容量：16

- 最大容量：2^30

- 加载因子：0.75

- 扩容机制：

  - 是Node节点的数量大于**阈值**时才扩容，而不是数组中被占位置的数量
  - 添加后判断，而不是添加前

  

#### 1.7和1.8的区别

1. 底层结构：
   - 1.7 数组+链表
   - 1.8 数组+链表+红黑树
     - 并不是链表长度大于8就转化为红黑树
     - 链表长度大于8，并且当前桶的长度不小于64的时候转化为红黑树
     - 如果链表长度大于8，但桶长度小于64，就扩容一次
2. 数组创建：
   - 1.7 new HashMap的时候创建数组
   - 1.8 第一次put方法的时候创建数组
3. 插入方式：
   - 1.7头插法，resize的时候会死循环
   - 1.8尾插法
4. resize时的方式
   - 1.7中resize的时候，所有Entry重新hash & length - 1
   - 1.8中resize，计算新参与进来的二进制高位是否为1
     - 如果为1，新位置=旧位置+原先的容量
     - 如果为0，新位置=旧位置
5. 1.7底层Entry数组，1.8Node数组



---



# 2. MySQL

## 1. 事务

- 定义：**事务由单独单元的一个或多个SQL语句组成，这个单元中，每个MySQL语句相互依赖的。**整个单独单元是<u>不可分割的整体</u> ，如果单元中某条SQL执行失败或产生错误，整个单元会回滚。所有收到影响的数据返回到事务开始以前的状态；如果单元中所有SQL语句都执行成功，则事务顺利执行。

  

### 1. ACID四大属性

1. **原子性**：事务是一个不可分割的工作单位，事务中操作要么全部执行，要么全部不执行
2. **一致性**：事务执行使数据库从一个一致状态切换到另一个一致状态
3. **隔离性**：一个事务的执行不能被其他事物干扰；一个事务内部的操作和使用的数据对并发的其他事物是隔离的，并发执行的各个事务之前不能相互干扰
4. **持久性**：一个事务一旦提交，对数据库数据的改变是永久的，接下来其他操作和数据库故障对此改变无影响

### 2. 使用过程

```sql
set autocommit=0;

start transaction; // 可选
语句...

commit; 提交事务

rollback;回滚事务
```



### 3. 并发问题与隔离级别

#### 1. 并发问题

同时运行多个事务，访问**数据库中相同的数据**时，会导致各种并发问题

- **脏读**：两个事务T1,T2，T1读取了T2更新但<font color=red>还没有提交</font>的字段，此时若T2回滚，T1读取的内容就是临时且无效的
- **不可重复读**：两个事务T1,T2，T1读取了一个字段，然后T2<font color=red>更新</font>了该字段后，T1再次读取同一个字段，值就不同了
- **幻读**：两个事务T1,T2，T1从一个表中读取一个字段，然后T2在此表中<font color=red>插入</font>了一些新的行，之后，T1再次读取同一个表，就会多出几行



#### 2. 隔离级别

**数据库事务隔离性**：数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免并发问题



数据库提供了4种**隔离级别**

- **READ UNCOMMITED(读取未提交数据)**：允许事务读取未被其他事务提交的变更；脏读，不可重复读，幻读都会出现
- **READ COMMITED(读已提交数据)**：只允许事务读取已经被其他事务提交的变更，可避免脏读，其他两种不能避免
- **REPEATABLE READ(可重复读)**：确保事务可以多次从一个字段读取相同的值；这个事务持续期间，禁止其他事务对这个字段更新；可以避免脏读和不可重复读
- **SERIALIZABLE(串行化)**：确保一个事务可以从一个表中读取相同的行；这个事务持续期间，禁止其他事务对该表插入，更新和删除；可避免所有并发问题，但性能十分辣鸡



> - Oracle支持2种事务隔离级别：READ COMMITED, SERIALIZABLE；默认为READ COMMITED
> - MySql支持4种：默认为REPEATABLE READ

![img](D:\Desktop\新建文件夹\clipboard.png)

#### 3. 操作

每启动一个mysql程序，就会获得一个单独的数据库连接，每个数据库连接都有一个全局变量@@tx_isolation，表示当前事务隔离级别

- 查看当前隔离级别

  ```sql
  SELECT @@tx_isolation;
  ```

- 设置当前连接的隔离级别

  ```sql
  set transaction isolation level read committed；
  ```

- 设置数据库系统的全局隔离级别；

  ```sql
  set global transaction isolation level read committed；
  ```

  







# Shein面经

## Java

### 1. 基础

1. 面向对象编程：

   一种设计思想，把对象作为程序基本单元，对象包含了数据和操作数据的函数；把程序视为一组对象的集合，每个对象可以接受其他对象发的消息，并且处理，程序的执行就是一系列消息在对象间的传递

2. Java三大特性：

   - 封装：隐藏对象的属性和实现细节，仅对外公开访问方法，控制读写访问级别；封装的关键在于，绝对不能让类中的方法直接地访问其他类的实例域。程序仅通过对象的方法与对象数据进行交互。
   - 继承：在已有类的基础上，增加新的方法或重写已有方法，从而产生新类
   - 多态：相同的事务，调用相同的方法，参数也相同时，但表现不同
     - 继承是多态实现的基础

3. 实现多态的三个必要条件

   - 继承
   - 重写
   - 向上转型：在多态中需要将子类的引用赋给父类对象，只有这样该引用才能够具备技能调用父类的方法和子类的方法。

4. 设计模式，挑重点讲一讲实现

5. 抽象类和借口的区别

   |     参数     |                 接口                 |            抽象类            |
   | :----------: | :----------------------------------: | :--------------------------: |
   |   默认方法   |        jdk1.8可以，之前不可以        |       可以实现默认方法       |
   |    继承性    | 多实现，并且接口和接口之间可以多继承 |            单继承            |
   | 可以有构造器 |         无构造器，不能实例化         |         可以有构造器         |
   |    关键字    |        implements，interface         |      extends，abstract       |
   |     main     |               没有main               |    有main方法，可以运行它    |
   |     速度     |    稍慢，需要时间找类中实现的方法    |             更快             |
   |   静态方法   |        jdk1.8可以，之前不可以        |                              |
   |  访问修饰符  |              全为public              |       可以有protected        |
   |   设计理念   |        like-a，是一种行为规范        | is-a，体现关系延续，模板设计 |

   

### 2. 多线程

1. 双重校验锁

   ``` java
   public class Singleton {  
       private volatile static Singleton singleton;  
       private Singleton (){}  
       public static Singleton getSingleton() {  
       if (singleton == null) {  
           synchronized (Singleton.class) {  
           if (singleton == null) {  
               singleton = new Singleton();  
           }  
           }  
       }  
       return singleton;  
       }  
   }
   ```

   - 第一次判断null：singleton对象已经创建，避免进入同步代码块，提升效率

   - 第二次判断null：此时singletion为null，线程A经过第一次判断singleton为null，此时线程B获得时间片，也判断singleton为null，因此进入同步代码快实例化；如果没有第二次判断，线程A就会又new一个对象，不符合单例模式要求

   - 为什么加volatile

     - 保证指令不会重排序

     - 可见性和有序性保证

       ```java
       singleton = new Singleton();  
       ```

       指令1：获取singleton对象的内存地址
       指令2：初始化singleton对象

       指令3：将这块内存地址，指向引用变量singleton。

      如果没有Volatile关键字，假设线程A正常创建一个实例，那么指定执行的顺序可能2-1-3，当执行到指令1的时候，线程B执行getInstance方法，获取到的，可能是对象的一部分，或者是不正确的对象，程序可能就会报异常信息。

### 3. 集合类

Java常用集合类

- Collection接口：
  - List接口
    - ArrayList
    - linkedlist
    - vector
  - Set接口
    - Hashset
    - treeset
- Map接口
  - hashMap
    - linkedHashMap
  - Treemap
  - hashtable
    - properties

### 4. JVM

1. 类加载过程

   - 加载：将class字节码文件加载到内存，把静态数据转换成方法区运行时数据结构，生成代表这个类的Class对象，作为方法区中类数据的访问入口
   - 链接
     - 验证：确保字节流包含信息符合当前虚拟机要求，保证类的正确性
       - 文件格式，元数据，字节码，符号引用
     - 准备：为类变量（不包括final，final在编译的时候就分配）分配内存并设置默认初始值，这些内存都在方法区中分配
     - 解析：常量池内的符号引用（常量名）替换为直接引用（地址）
   - 初始化：类构造器<clinit\>，收集类变量的赋值动作和静态代码块并执行

2. 运行时数据区

   线程共享：

   - 方法区：类信息，常量，静态变量等
   - 堆：对象实例，数组

   线程独立：

   - 虚拟机栈：Java方法（局部变量表，操作数栈，动态链接，方法出口）
   - 程序计数器
   - 本地方法栈

## 数据库

1. 事务隔离级别
   - read uncommitted
   - read committedread
   - repeatable
   - serializable
2. redis存储类型和底层数据结构
   - list：双向链表
   - set：value为null的hash表
   - String：最基本数据类型
   - hash：String类型的k-v映射表，类似hashmap
   - zset：hash和跳跃列表
