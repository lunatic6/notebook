# 2. 类加载子系统

## 1. 内存模型概述

![内存模型.png](D:\Desktop\markdown\尚硅谷jvm\内存模型.png)

![内存详细1.png](D:\Desktop\markdown\尚硅谷jvm\内存详细1.png)

![内存详细2.png](D:\Desktop\markdown\尚硅谷jvm\内存详细2.png)

![内存详细3.png](D:\Desktop\markdown\尚硅谷jvm\内存详细3.png)

## 2. 类加载子系统概述

![类加载子系统.png](D:\Desktop\markdown\尚硅谷jvm\类加载子系统.png)

![类加载器角色.png](D:\Desktop\markdown\尚硅谷jvm\类加载器角色.png)

## 3. 类加载过程

![类加载过程.png](D:\Desktop\markdown\尚硅谷jvm\类加载过程.png)

![类加载过程1.png](D:\Desktop\markdown\尚硅谷jvm\类加载过程1.png)



### 1. 加载

![加载.png](D:\Desktop\markdown\尚硅谷jvm\加载.png)

![加载1.png](D:\Desktop\markdown\尚硅谷jvm\加载1.png)

### 2. 链接（验证，准备，解析）

![链接.png](D:\Desktop\markdown\尚硅谷jvm\链接.png)

![准备.png](D:\Desktop\markdown\尚硅谷jvm\准备.png)

![初始化.png](D:\Desktop\markdown\尚硅谷jvm\初始化.png)

### 3. 类变量的赋值可以在声明前

因为先prepare，再初始化，而prepare就声明了类变量	

![类变量的赋值.png](D:\Desktop\markdown\尚硅谷jvm\类变量的赋值.png)

​	注意：没有static变量和static代码块，clinit就不会执行

## 4. 类加载器

![类别.png](D:\Desktop\markdown\尚硅谷jvm\类别.png)

![类别1.png](D:\Desktop\markdown\尚硅谷jvm\类别1.png)

除了bootstrap，其他都属于自定义加载器，因为都继承了classloader



![代码示例.png](D:\Desktop\markdown\尚硅谷jvm\代码示例.png)

![代码示例2.png](D:\Desktop\markdown\尚硅谷jvm\代码示例2.png)

小总结：

- 用户自定义类，都是系统类加载器加载
- java核心库类都是引导类加载器加载

## 双亲委派机制