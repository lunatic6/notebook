

注：\r 表示回车，但不换行，从当前行的顶头开始输出，覆盖掉以前的内容

# 1. 概述

## 1. 注释

1. 行注释

   - 语法：// 注释内容

2. 块注释（多行注释）

   - 语法： / *

     ​      		注释内容  

     ​			*/

3. 注：
   1. 两种注释中，注释的文字不会被Go编译器执行
   2. 块注释里面不允许有块注释嵌套

## 2. 代码规范

1. 官方推荐使用**行注释**
2.  缩进：
   1. 可以使用tab和shift+tab调整缩进
   2. 也可以使用gofmt进行格式化
      1. 命令行：gofmt -w go文件
3. 运算符两边各加一个空格
4. 行长：
   - 一行最长不超过80个字符，超过的请换行展示

## 3. DOS常用指令

### 1. 目录操作

1. 查看当前目录包含的文件和目录：dir
2. 切换目录：`cd 绝对/相对路径`
   1. 回到上级目录：`cd ..`
   2. 回到根目录：`cd \`
   3. 

3. 新建目录：`md 目录1 目录2`
4. 删除目录：
   1. 删除空：`rd 目录`
   2. 删除目录以及下面的子目录和文件，不带询问：`rd /q/s 目录`
      - q代表不用询问，s代表层级
   3. 带询问：`rd /s 目录`

### 2. 文件操作

1. 新建或追加内容到文件：`echo 内容 > 文件名`

2. 复制文件：`copy 文件 路径\指定文件名（可选）`
3. 移动：`move 文件名 路径`

4. 删除：`del 文件名`， 例如`del *.txt`删除所有txt文件

### 3. 其他操作

1. 清屏：cls
2. 退出：exit

# 2. 变量

## 1. 注意事项

1. Golang中变量使用的三种方式：

   1. 指定变量类型，声明后如果不赋值，使用默认值
   2. 根据值，自行判断变量类型（类型推导）
   3. 省略关键字`var`，注意`:=` 左侧的变量不应该是已经声明过的，否则会导致编译错误。
      - 注意冒号不能省略

2. 支持**多变量声明**

   1. 局部变量		

   ```go
   // 方式1
   var n1,n2,n3 int
   
   // 方式2
   var n1, name ,n3 = 100, "tom", 900
   
   // 方式3，类型推导
   n1, name, n3 := 100, "tom", 900
   ```

   2. 全局变量（函数外部声明）

      ```go
      var (
      	n3 = 300
          n4 = 900
          name = "tom"
      )
      ```

      - 注：函数外不能使用`:=`

3. 该区域的数据值，可以在**同一类型范围**内**不断变化**。（变量值可以改变，但不能变成不是自己数据类型的值）

4.  变量在同一个作用于内不能重名

5. 变量=变量名+值+数据类型，变量三要素

6. 变量如果没有赋初值，编译器会使用默认值。（小数默认也为0）

7. +号

   1. 两边都是数值型，做加法运算
   2. 都是字符串，做字符串拼接

## 2. 数据类型

1. 数据类型类别
   1. 基本数据类型
      1. 数值型
         1. 整数类型（int，int8，int16，int32，int64，uint，uint8，uint16，uint32，uint64，byte）
         2. 浮点类型（float32，float64——相当于java中double）
      2. 字符型（没有专门的字符型，用byte保存单个字母字符）
         1. 注：不能用来保存汉字，go中使用utf-8编码，一个汉字3个字节
      3. 布尔型（bool）
      4. 字符串：官方将string归属到基本数据类型
   2. 派生/复杂数据类型
      1. 指针
      2. 数组
      3. 结构体（struct），类似于类
      4. 管道（Channel）
      5. 函数（也是一种类型）
      6. 切片（slice)
      7. 接口（interface）
      8. map

### 1. 基本数据类型

#### 1. 整数

 1. int和unit，如果不接数字，则占用空间取决于系统，32位系统4字节，64位系统8字节

 2. rune，有符号

     	1. 存储空间和int32一样
          	2. 等价于int32，表示一个unicode码，用户处理带有中文的字符串

 3. byte，无符号

     	1. 与unit8等价，0-255
          	2. 存储字符时，用byte

 4. **使用细节**

     1. 整数默认声明为int

     2. 在程序中**查看某个变量的字节大小和数据类型**

        - 字节大小：unsafe.Sizeof(变量)

        - 数据类型：在fmt.Printf中使用`%T`

          

        - ```go
          // 举例
          var n int 64 = 10
          fmt.Printf("n的类型是 %T  n占用的字节数是 %d", n, unsafe.Sizeof(n))
          ```

    3. 整形变量使用时，遵循保小不保大原则
       
       1. 即保证程序正确运行下，尽量使用占用空间小的数据类型 
4. bit：计算机中最小的存储单位；byte：计算机中基本存储单元。1byte=8bit
   
    

#### 2. 小数/浮点型

1. 可能有精度损失
2. **使用细节**
   1. 具有固定的范围和字段长度，不受OS的影响
   2. 默认声明为float64
   3.   .123等价于0.123
   4. 支持科学计数法
      1. 通常推荐使用float64，精度更高

#### 3. 字符类型

	1. 没有专门的字符型，用byte保存单个字母字符
	2. 传统的字符串由字符组成，**Go的字符串由字节组成**
	3. 直接输出byte时，输出的是对应字符的码值
	  	1. 希望输出对应字符，需要使用格式化输出`%c`
	  	2. 可用**int存汉字字符**，`%c`输出字符，`%d`输出码值
	       	1. 也就是字符对应的ASCII值大于255，可以用int保存
	4. **使用细节**
	  	1. 字符常量：用单引号括起来的单个字符
	  	2. 允许使用转义字符将后面的字符变为特殊字符，比如'\n'
	  	3. 字符使用utf-8编码
	       	1. 英文1字节，汉字3字节
	  	4. Go中字符的本质是一个整数，直接输出时，是该字符对应utf-8编码的码值
	  	5. 可以直接给某个变量赋一个数值，然后格式化输出`%c`，会输出该数字对应的unicode字符
	  	6. 字符类型可以进行运算，因为都有对应的unicode编码
	5. 字符类型本质
	  	1. 存储：字符—>码值—>二进制—>存储
	  	2. 读取：二进制—>码值—>字符—>读取

#### 4. 布尔类型

1. 占一个字节
2. 只允许取true或false，不允许为空
3. **不能用0或非0整数替代false和true**

#### 5. 字符串类型

1. Go字符串是由单个字节连接起来，使用UTF-8编码表示Unicode文本

2. Go中字符串是不可变的，一旦赋值，不能修改

   1. ```go
      var str = "hello"
      str[0] = 'a' //这里就不能修改str的内容
      ```

3. 字符串两种表示形式

   1. 双引号，会识别转义字符
   2. 反引号，以字符串的**原生形式**（字符串是什么就输出什么，没有各种功能性字符的效果）输出，包括换行和特殊字符，可以实现防止攻击，输出源代码等效果。

4. 字符串可以用加号拼接

   1. 一个拼接操作很长时，可以分行写。但是加号要留在上一行

   2. ```go
      var str = "hello" + "bob" + 
      	"hello" + "bob" // 正确
      
      
      var str = "hello" + "bob" 
      	 + "hello" + "bob" // 错误
      ```

      

#### 6. 关于基本数据类型

##### 1. 默认值

​	go中数据类型都有默认值，又叫零值

1. 整型：0
2. 浮点型：0
3. 字符串：""
4. 布尔值：false

##### 2. 基本数据类型的转换

1. 与java/C不同，Go中数据类型**不能自动转换**
2. 基本语法：
   - 利用`T(v)`将v转换为类型T
     - T为数据类型
     - v为需要转换的变量

3. 细节说明

   1. 数据类型转换时，高精度和低精度都可以互相转换(但高转低可能发生数据溢出)

   2. 被转换的是**变量存储的数据（也就是值）**，变量本身的数据类型没有变化

      1. ```go
         var i int32 = 100
         var n int64 = int64(i)
         // i的数据类型并没有改变
         // 只是把i的值转换为int64了
         ```

   3. 注意：转换中，例如将int64转为int8，编译不会报错，只是转换结果是按照2进制**溢出处理**。因此转换时要考虑数据类型的范围

      - ```go
        var n1 int64 = 999999
        var n2 int8 = int8(n1)
        // 此时n2的值是63，而不是999999
        ```

4. 小例题

   - ```go
     var n1 int32 = 12
     var n2 int8
     var n3 int8
     
     n2 = int8(n1) + 127 // 编译通过，但是溢出
     n3 = int8(n1) + 128 // 编译无法通过
     // 128不是int8，不能赋给n3
     ```

   - 

#### 7. 基本数据类型和String转换

##### 1. 基本数据转string

1. 方式一：`fmt.Sprintf("%参数"，表达式)`
   - 参数需要和表达式的数据类型相匹配
   - 返回转换后的字符串
2. 方式二：使用strconv包的函数
   1. FormatInt
   2. FormatFloat
   3. FormatBool
   4. Itoa

##### 2. String转基本数据类型

1. 使用strconv包的函数
   1. ParseBool
   2. ParseInt
   3. ParseFloat

2. 使用细节
   1. 要确保String类型能够转成有效的数据。比如可以把“123”转为整数，但是不能把“hello”转成整数，如果这样，将把它转成0；如果是bool，就默认为false



### 2. 复杂数据类型

#### 1. 指针

1. 基本介绍 
   1. 基本数据类型：变量存的就是值，也叫值类型
   2. 获取变量的地址，用`&`。例如：`var num int`，`&num`可以获取num的地址
   3. 指针类型：变量存的是一个地址，此地址指向的空间存的才是值。例如：`var ptr *int = &num`
      - ptr是一个指针变量
      - 类型是 `*int`
      - 本身的值是`&i`
      - 注：指针也有自己的地址（因为它也需要空间保存变量的地址）
   4. 获取指针类型所指向的值，使用：`*`。例如：`var *ptr int`, 使用`*ptr`获取p指向的值

<img src="D:\Desktop\Markdown\Golang\指针.png" alt="image-20210424163352085" style="zoom: 67%;" />

<img src="D:\Desktop\Markdown\Golang\指针2.png" alt="image-20210424163656729" style="zoom: 67%;" />

2. 使用细节
   - 值类型，都有对应的指针类型，形式为`*数据类型`。int对应的就是`*int`，float32对应的就是`*float`。

### 3. 值类型和引用类型

1. **值类型**：
    1. 基本数据类型int系列，float系列，bool，string
    2. 数组
    3. 结构体struct
 2. **引用类型**
     1. 指针
     2. slice切片
     3. map
     4. 管道channel
     5. interface等

3. 两者的使用特点
   1. 值类型：**变量直接存储值**，内存通常在**栈**中分配
   2. 引用类型：变量存储的是一个**地址**，这个地址对应的空间才是真正存储的数据。内存通常在**堆**上分配，当没有任何变量引用这个地址时，该地址的数据空间就变成一个垃圾，由GC回收。

## 3. 标识符

### 1. 基本概念



1. 概念

   1. Go中对各种变量，方法，函数等命名时使用的字符序列称为标识符
   2. 凡是自己可以起名字的地方，都叫标识符

2. 命名规则

   1. 由26个英文字母的大小写，0-9，下划线_构成
   2. 数字不能开头
   3. 严格区分大小写
   4. 不能包含空格
   5. 下划线`_`是一个特殊的标识符，称为空标识符
      1. 可以代表其他任何标识符，但是它对应的值会被**忽略**（比如，希望忽略某个返回值，因为go中的变量必须要被使用。如果一个函数返回两个值，但是我们只想用其中一个，就可以用`_`接收不想用的那个值）
      2. 因此它仅能作为占位符使用，不能作为标识符使用
   6. 不能以系统保留关键字（一共25个）作为标识符，如break，if等

3. 保留关键字

   <img src="D:\Desktop\Markdown\Golang\保留关键字" alt="image-20210424180910538" style="zoom:50%;" />

4. 预定义标识符

   <img src="D:\Desktop\Markdown\Golang\预定义标识符" alt="image-20210424181936404" style="zoom:50%;" />

### 2. 标识符命名规范

 	1. 注意事项
 	  	1. 包名：保持package的名字和目录名一直，尽量采取有意义的包名，且简短。不要和标准库冲突
 	  	2. 变量名，函数名，常量名采用驼峰法
 	  	3. 若变量名，函数名，常量名首字母大写，则可以被其他的包访问；否则，只能在本包中使用
 	      - 可以简单理解成，首字母大写为公有，反之为私有

## 4. 变量作用域

1. 说明：
   1. 局部变量：函数内部定义/声明的变量，作用域仅限于函数内部
   2. 全局变量：函数外部声明/定义的变量，作用域在整个包都有效；如果首字母大写，则作用域是整个程序
   3. 如果是在代码块中定义，比如循环，那么作用域就是该代码块



# 3. 运算符

	## 1. 算术运算符

<img src="D:\Desktop\Markdown\Golang\算数运算符.png" alt="image-20210425132922113" style="zoom: 67%;" />

1. 除号`/`：两边都是整数，结果只保留整数
2. 取模`%`：
   1. 公式：`a % b = a - a / b * b`
3. `++`和`--`
   1. 是语句，不是表达式，不能这样用：
      - `b := a++ `
      - `b := a--`
   2. 只能写在**变量后面**

## 2. 关系运算符

<img src="D:\Desktop\Markdown\Golang\关系运算符.png" alt="image-20210425134108741" style="zoom: 67%;" />



## 3. 逻辑运算符

<img src="D:\Desktop\Markdown\Golang\逻辑运算符.png" alt="image-20210425134316346" style="zoom: 67%;" />



## 4. 赋值运算符

<img src="D:\Desktop\Markdown\Golang\赋值运算符.png" alt="image-20210425134611508" style="zoom:67%;" />

<img src="D:\Desktop\Markdown\Golang\赋值运算符2.png" alt="image-20210425134651914" style="zoom:67%;" />



1. 经典面试题，不使用中间变量，交换两个变量的值

   - ```go
     a = a + b
     b = a - b
     a = a - b
     ```

   - 当然，go中可以直接`a,b = b,a`



## 5. 运算符优先级

<img src="D:\Desktop\Markdown\Golang\运算符优先级.png" alt="image-20210425135420720" style="zoom:67%;" />



## 6. 位运算符

<img src="D:\Desktop\Markdown\Golang\位运算符.png" alt="image-20210425135610722" style="zoom:67%;" />



## 7. 其他运算符

1. 取地址`&`
2. 指针`*`
3. 特别说明：go中没有三目运算符` :?`



## 获取用户输入

1. **fmt.Scanln(&变量名)**：获取一行的输入

   - 参数是变量的地址，直接把值放到地址上

2. **fmt.Scanf(格式，&变量名)**：根据格式获取输入

   - ```go
     fmt.Scanf("%s %d %f %t", &name, &age, &salary, &isPass)
     ```

   - 输入的时候要严格按照上面的顺序输入，并且按照上面的要求添加空格



# 4. 流程控制

## 1. if-else

1. 语法

   - ```go
     if 条件表达式 {
         代码块
     } else if 条件表达式{
         代码块
     } else { // 注： else必须和上一个花括号在同一行
         代码块
     }
     ```

   - 注：即使代码块只有一句话，也需要加外面的花括号

2. Go支持在if的条件判断位置，直接定义一个变量，比如

   - ```go
     if age := 20; age > 18 {
     	代码块
     }
     ```

   - 注：这个变量的作用域，只能在该条件逻辑块内



## 2. switch

1. Go中的switch，case后面**不需要加break**（默认每一个case满足后，都break）

2. 语法

   - ```go
     switch 表达式{
     	case 表达式1,表达式2,...:
         	语句块
         case ...:
         	语句块
         default:
         	语句块
     }
     ```

   - 注：case后的表达式可以有多个，使用 逗号 间隔

3. 使用细节

   1. case后各个表达式的数据类型，要和switch表达式的数据类型一致

   2. case后可以带多个表达式，逗号间隔

   3. case表达式如果是常量值（字面量），则要求不能重复

      - 比如：第一个case的表达式有常量5，其他的表达式就不能有常量5了

   4. switch后面也可以不接表达式，当作if-else使用

      - ```go
        switch{
        	case age == 10:
        		语句块
            case age == 20:
                语句块
        }
        ```

   5. switch后也可以直接声明/定义一个变量，以分号结束，但**不推荐**

   6. switch穿透-fallthrough

      - 如果在case语句块后增加fallthrough，则会继续执行下一个case（只能穿透一层），也叫switch穿透
      - 是直接执行下一个case的语句块
      - 紧接的下一个case条件里，不允许定义常量/变量
      - 不建议使用

   7. Type Switch：switch语句还可以被用于type-switch来判断某个interface变量中，实际指向的变量类型



## 3. for

1. 语法

   - ```go
     for 循环变量初始化; 循环条件; 循环变量迭代{
         循环语句
     }
     ```

2. 注意事项

   1. 第二种语法

      - 将变量初始化和变量迭代写道其他位置

      - ```go
        for 循环判断条件 {
            循环语句
        }
        ```

   2. 第三种

      - ```go
        for {
            循环语句
        }
        ```

      - 死循环，通常需要配合break

      - java中这样写是错的，go允许这样写

   3. 提供了**for-range**方式，方便遍历字符串和数组

      - 传统遍历字符串

        - ```go
          var str string = "hello golang"
          
          for i := 0; i < len(str); i++{
              fmt.Println(str[i])
          }
          
          ```

        - 如果字符串中有中文，传统写法是按照字节遍历的，则中文会乱码

        - 解决：需要将str转成[]rune切片

      - for-range

        - ```go
          var str string = "hello golang"
          
          
          
          for index, val := range str{
              fmt.Println(index, val)
          }
          ```

        - 不会出现乱码，因为for-range是按照字符遍历的，但index是标记的字节的起始索引

          - 例如”上海“，海对应的index是3

      

3. while和do...while实现

   1. go没有while和do...while，可以用for实现

   2. for实现while

      - ```go
        循环变量初始化
        for{
            if 循环条件表达式 {
                break 
            }
            循环语句
            循环变量迭代
        }
        ```

   3. for实现do...while

      - ```go
        循环变量初始化
        for{
            循环语句
            循环变量迭代
            if 循环条件表达式 {
                break 
            }
        }
        ```

   4. 打印空心金字塔

      ```go
      var totalLevel int = 20
      
      for i:=1; i <= totalLevel; i++ {
          for k := 1; k <= totalLevel - i; k++{
              fmt.Print(" ")
          }
      
          for j := 1; j <= i * 2 - 1; j++{
              if j == 1 || j == i * 2 - 1 || i == totalLevel{
                  fmt.Print("*")
              }else {
                  fmt.Print(" ")
              }
          }
      
          fmt.Println()
      }
      ```

      

## 4. break

1. 生成随机数
   1. `time.Now().Unix()`：返回一个从1970标准日到现在的秒数
   2. `rand.Seed(time.Now().Unix()):`为设计随机数生成种子
   3. `rand.Intn(n)`：返回一个[0,n)的伪随机数

2. 使用细节

   1. 在多层嵌套的语句块中，可以通过**标签**指明要终止的是哪一层语句块

      - ```go
        label1: {
            label2: {
                lable3 : {
                    break label2 // 直接跳到第二层循环结束
                }
            }
        }
        ```

        

## 5. continue

1. 结束**本次**循环，继续下一次循环
2. label用法和break一样

## 6. goto

1. 无条件转移到程序中指定的行

2. 语法：

   - ```go
     goto label
     ...
     lable: statement
     ```

3. 不主张使用





# 5. 函数、包、错误处理

## 1. 函数

1. 定义：为完成某一功能的程序指令（语句）的集合

2. 语法：

   - ```go
     func 函数名 (形参列表) (返回值列表){
         执行语句...
         return 返回值列表
     }
     ```

   - go函数支持返回多个值

   - 返回多个值时，如果希望忽略某个返回值，则使用`_`符号表示占位忽略

   - 如果返回值只有一个，返回值类型列表可以不写括号`()`

---

### 1. 使用细节

 1. 形参列表和返回值列表都可以是多个

 2. 形参列表和返回值列表的数据类型，可以是值类型和引用类型

     	1. 值传递和引用传递，传给函数的都是变量的副本
         - 区别在于，值传递值的拷贝，引用传递地址的拷贝
         - 一般来说，**地址拷贝效率高**，因为数据量小，值拷贝取决于拷贝数据的大小

 3. 命名遵循标识符命名规范，不能以数字开头

      - 首字母大写相当于public

      - 小写相当于private

 4. 基本数据类型和**数组**（java不是），默认**值传递**

    - 即函数内修改，不会影响原来的值

5. 如果希望函数内部变量可以修改函数外的变量，可以传入变量地址，在函数内部用指针

6. 不支持重载

7. Go中，**函数也是一种数据类型**。可以赋值给一个变量，则该变量就是函数类型的变量。通过该变量可以调用函数

      - ```go
        func getSum(n1 int, n2 int) int {
        	return n1 + n2
        }
        
        func main() {
        
        	a := getSum // 此时a就是函数变量
        	
        	res := a(10, 40) // 可以通过a调用函数
        	
        	fmt.Println(res)
        }
        ```

8. 既然函数是数据类型，那么Go中函数可以作为形参，并且调用

      - ```go
        func getSum(n1 int, n2 int) int {
        	return n1 + n2
        }
        
        func myFun(funvar func(int,int) int, num1 int, num2 int){
            return funvar(num1, num2)
        }
        
        func main() {
        
        	a := getSum // 此时a就是函数变量
        	
        	res := a(10, 40) // 可以通过a调用函数
        	
            res2 := myFun(getSum, 20,30) // 函数作为参数传给myFun这个函数
        }
        ```

9. 为了简化数据类型定义，Go支持自定义数据类型

      - 语法：type 自定义数据类型名 数据类型 
        - 相当于别名
      - 案例：`type myInt int `
        - 此时myInt等价于int来使用
      - 案例：`type mySum func(int, int) int`
        - muSum相当于一个函数类型func(int,int) int
      - **注意**：虽然上面myInt和int都是int类型，但是Go认为他们不相同，是两种类型

10. 支持对返回值命名

       - ```go
         func getSum(n1 int, n2 int) (sum int, sub int){
             sum = n1 + n2
             sub = n1 - n2
             return
         }
         ```

       - 相当于直接定义了sum和sub，也规定好了返回的顺序

11. 使用`_`标识符，忽略返回值

12. 支持可变参数

     1. 支持0到多个参数
    
        - ```go
          func sum(args... int) int{    
          }
          ```
    
     2. 支持1到多个参数
    
        - ```go
          func sum(n1 int, args... int) int{
          }
          ```
    
     3. 说明
    
        - agrs是slice切片，通过args[index]可以访问到各个值
        - 必须放到形参列表的最后



### 2. init函数

 	1. 介绍
 	 - 每一个源文件都可以包含一个init函数，该函数会在**main函数之前被调用**
 	 - 通常用来做一些初始化的工作
 	2. 使用细节 
 	    	1. 如果一个文件同时包含全局变量定义，init函数和main函数，则执行的流程是**变量定义->init函数->main函数**
 	     2. 最主要作用，就是完成初始化工作
 	     3. 导入其他包时
 	       1. 首先执行被引用包的全局变量定义
 	       2. 然后调用被引用包的init函数
 	       3. 最后执行本程序的流程
 	       4. 注：相当于被引用的这些包里面的流程，是在import时就开始执行了

### 3. 匿名函数

1. 介绍

   - 如果某个函数，只希望用一次，可以使用匿名函数
   - 也可以实现多次调用

2. 使用方式

   1. 定义时，就直接调用

      - 这种方式只能用一次

      - ```go
        res := func(n1 int, n2 int) int {
            return n1 + n2
        }(10, 20) // 此处直接传参
        ```

      - 如果是没有return的匿名函数，直接在最后加上`()`就等于调用了

   2. 匿名函数赋给一个变量（函数变量），再通过该变量来调用

      - ```go
        a := func(n1 int, n2 int) int {
            return n1 + n2
        }
        // 此时a就是函数类型的变量
        ```

   3. 全局匿名函数：将匿名函数赋给一个全局变量，那么这个匿名函数就成为一个全局匿名函数



### 4. 闭包

1. 介绍：闭包就是**一个函数**和**其相关的引用环境**组合的一个**整体**（实体）

2. 案例1：

   1. ```go
      // 累加器
      func AddUpper() func(int) int{
      	n := 10
      	return func(x int) int{
      		n += x
      		return n
      	}
      }
      
      func main() {
      
      	a := AddUpper()
      	fmt.Println(a(1)) 	//11
      	fmt.Println(a(2)) 	//13
      	fmt.Println(a(3)) 	//16
      }
      ```

   2. 对上面代码的说明：

      - AddUpper是一个函数，返回的数据类型是`func(int) int`
      - 闭包的说明：上面的累加器返回一个匿名函数，但是这个匿名函数引用到函数外的n，因此这个匿名函数可以这样理解：闭包是类，函数是操作，n是字段。函数和它使用到的n构成闭包
      - 当我们反复调用a函数时，因为n是初始化一次，因此每调用一次就类型累加。
      - 搞清楚闭包的关键，就是要分析出返回的函数，它使用到了哪些变量。因为函数和它引用到的变量共同构成闭包。

3. 案例2：

   <img src="D:\Desktop\Markdown\Golang\闭包案例.png" alt="image-20210427233408270" style="zoom: 67%;" />

   1. ```go
      func makeSuffix(suffix string) func (string) string {
      	
          // 和变量suffix构成闭包
      	return func(name string) string {
      		if !strings.HasSuffix(name, suffix) {
      			return name + suffix
      		}
      		return name
      	}
      }
      
      func main() {
      
      	f := makeSuffix(".jpg")
      	fmt.Println(f("winter"))
      	
      }
      ```

   2. 说明：

      - 返回的函数和makeSuffix函数的suffix变量组合成一个闭包，因为返回的函数引用到suffix这个变量
      - 体会闭包的好处，如果使用传统函数，也可以实现该功能，但传统方法每次调用函数都需要传入指定的后缀名。而**闭包因为可以保留上次引用的某个值，所以我们传入一次之后就可以反复使用。**



### 5. defer

1. 介绍：函数中，我们经常需要创建资源（比如数据库连接，文件句柄，锁等），为了函数执行完毕后，**及时释放资源**，提供了defer（延时处理，**defer栈**）

   - 主要价值：**及时释放资源**

2. 入门案例

   - ```go
     func sum (n1 int, n2 int) int {
         
         // 下面两个语句入defer栈
     	defer fmt.Println("n1 = ", n1)
     	defer fmt.Println("n2 = ", n2)
     
     	res := n1 + n2
     	fmt.Println("Fine n3 = " ,res)
     	return res
     }
     
     func main() {
     
     	res := sum(10 ,20)
         // 执行完函数，defer栈的语句出栈并执行
         
     	fmt.Println("res = ", res)
     
     }
     
     // 上面的输出
     Fine n3 =  30
     n2 =  20
     n1 =  10
     res =  30
     ```

3. 细节说明

   1. go执行到defer时，不会理解执行defer后的语句，而是将defer后的语句压入一个栈中（可以理解为一个defer栈），然后继续执行函数下一个语句

   2. 函数执行完毕后，从defer栈，依次 从栈顶取出语句执行

   3. defer将语句放入到栈时，**也会将相关的值拷贝同时入栈**

      - ```go
        func sum (n1 int, n2 int) int {
            
            // 下面两个语句入defer栈
        	defer fmt.Println("n1 = ", n1)
        	defer fmt.Println("n2 = ", n2)
        	
            n1++ // 此处的操作，不会影响上面defer语句里面的n1
            n2++ // 和n2的值
        	res := n1 + n2
        	fmt.Println("Fine n3 = " ,res)
        	return res
        }
        ```

   4. 最佳实践：defer的主要价值就是为了及时释放资源

      - ```go
        func test(){
            file = openfile(文件名) // 或者是数据库连接，锁资源
            defer file.close()
            // 也就是说我们获得文件对象后，可以立即写释放资源的代码
            // 防止后面忘记释放资源   
        }
        ```

      - defer后，可以继续使用创建的资源

      - 当函数执行完毕后，系统会依次从defer栈中取出语句，释放资源。

      - 这种机制，可以很简洁的帮助程序员，省去为关闭资源的时机而考虑的事情

### 6. 字符串常用函数

1. **统计字符串长度，按字节**：`len(str)`
2.  **遍历字符串，同时处理中文问题**
   1. 非中文字符时，直接str(i)就可以获得里面的字符
   2. 有中文时，需要字符串先转化为rune切片，`tmp=[]rune(str)`，然后tmp[i]就可获得里面的字符

3. **字符串转整数**：`n, err := strconv.Atoi(字符串)`
   - 会同时返回一个err，表示是否转换成功
4. **整数转字符串**：`str = strconv.Itoa(数字)`
5. **字符串转[]byte**：`var bytes = []byte(字符串)`
6. **[]byte 转字符串**：`str = string([]byte{97,98,99})`
7. **10进制，转2,8,16进制**：`str = strconv.FormatInt(数字，进制2/8/16)`
   - 返回对应的字符串
8. **查找子串是否在指定的字符串中**：`strings.Contains(目标字符串，子串)`
   - 返回布尔值
9. **统计字符串中有几个不重复的子串**：`num = strings.Count(目标字符串，子串)`
10. **不分大小写，比较字符串**：`strings.EqualFold(str1,str2)`
    - 返回布尔值
    - `==`是区分字母大小写的
11. **返回子串在字符串第一次出现的index值，如果没有返回-1**：`strings.Index(目标字符串，子串)`
12. **返回子串在字符串最后一次出现的index值，如果没有返回-1**：`strings.LastIndex(目标字符串，子串)`
13. **将指定的子串替换成另外一个子串**：`strings.Replace(目标串，被替换子串，替换子串，n)`
    - n表示想替换几个，-1表示全部替换
    - 函数返回新字符串，原目标串并没有被改变
14. **按照指定的某个字符（分割标识），将一个字符串拆分成字符串数组**：`strings.Split(目标串，标识)`
15. **字符串大小写变换**：
    - `strings.ToLower(str)`
    - `strings.ToUpper(str)`

16. **去掉字符串某位置的字符**
    - 左右两边的空格：`strings.TrimSpace()`
    - 左右两边指定字符：`strings.Trim(str, str1)`
    - 左边指定字符：`strings.TrimLeft(str, str1)`
    - 右边指定字符：`strings.TrimRight(str, str1)`

17. 判断字符串是否以指定的字符串开头/结尾
    1. 开头：`strings.HasPrefix(str,str)`
    2. 结尾：`strings.HasSuffix(str,str)`



### 7. 时间和日期函数

1. 需要引入`time`包

2. `time.Time`数据类型，用于表示时间

3. **获取当前时间：**`now := time.Now()`

   - now的类型就是`time.Time`
   - 输出：`2021-04-28 23:26:24.8681182 +0800 CST m=+0.001994101`
   - 通过time.Time类型的方法，可以拿到年月日时分秒等数据
     - `now.Year()`
     - `now.Month()`
     - `now.Day()`
       - 注：得到的为英文的月份，使用int()可以转为数字
     - `now.Hour()`
     - `now.Minute()`
     - `now.Second()`

4. 格式化日期时间

   1. 第一种方式：使用上面的方法，结合Sprintf
   2. 第二种方式：使用`now.Format`
      - `now.Format("2006/01/02 15:04:05")`
      - 注意：
        - "2006/01/02 15:04:05"字符串的各个数字，是**固定的**，**必须这样写**
        - 年，月，日，时，分，秒都可以单独出现，或者自由组合

5. 时间常量

   1. time里面定义的常量

   2. 作用：在程序中获取指定时间单位的时间，比如100毫秒
   
   3. ```go
   const (
          Nanosecond  Duration = 1
       Microsecond          = 1000 * Nanosecond
          Millisecond          = 1000 * Microsecond
          Second               = 1000 * Millisecond
          Minute               = 60 * Second
          Hour                 = 60 * Minute
      )
      ```
   
   4. 使用场景：结合time.Sleep(时间间隔)
   
6. 休眠：`time.Sleep(d Duration)`，利用上面的时间常量

7. 获取当前unix时间戳和unixnano时间戳
   1. 作用：获取随机数字
   2. `time.Now().Unix()`返回从1970标准时间到现在，所经过的时间，单位秒
   3. `time.Now().UnixNano()`返回从1970标准时间到现在，所经过的时间，单位纳秒



### 8. 内置函数

 	1. `len`：用来求长度，比如string，array，slice，map，channel
 	2. `new`：用来分配内存，主要用来分配**值类型**，比如int，float32，返回的是指针
 	 - `num := new(int)`，num是一个*int
 	3. `make`：用来给**引用类型**分配内存，比如channel、map、slice。
 	4. 后续补充

## 2. 包

1. **本质**：创建不同的文件夹，存放程序文件

2. go的每一个文件，都是属于一个包的；也就是说go以包的形式来**管理文件和项目目录结构**
3. **三大作用**
   1. 区分相同名字的函数、变量等标识符
   2. 当程序文件很多时，可以很好的管理项目
   3. 控制函数、变量等访问范围，即作用域

4. **相关说明**
   
   1. 打包：`package 包名`
2. 引入：`import 包路径`
   
5. **使用细节**：

   1. 文件的包名**通常**和文件所在的文件夹名一致，一般为小写字母
      - 注：比如在main文件夹下的程序，写成`package abc`，在其他程序导入main包`import "main"`，之后是用`abc.函数名调用的`
      - **也就是说，package规定了调用自己函数时的前缀，import的导入的是你这个程序的路径**

   2. import包时，路径从gopath的src下开始，不用带src，编译器自动从src下开始引入

   3. 为了让其他包的文件，可以访问本包函数，函数首字母名字需要大写

      1. 首字母大写类似于public

   4. 访问其他包函数：`包名.函数名`

   5. 若包名较长，可以给包取别名

      - ```go
        import (
        	别名 路径
        )
        ```

      - 起别名后，不能再使用原包名

   6. 同一包下，不能有相同的函数名、全局变量名

      - **不同文件，但是相同包，也不允许**

   7. 如果要编译成一个可执行文件，需要把包声明为main。如果写一个库，包名随意。

      1. 编译项目时，要编译main包所在的文件夹

      2.  编译时，在GOPATH目录下

         ```go
         go build -o 希望编译生成的目录和程序名 main包所在路径
         
         其中-o和后面路径是可选的，没有的话默认生成main.exe
         ```





## 3. 错误处理

### 1. 基本介绍

1. Go追求简洁优雅，因此不支持传统的`try catch finally`
2. 引入的处理方式为：`defer，pannic，recover`
3. 简单描述上面的使用场景：Go抛出一个panic的异常，**在defer中，通过recover捕获这个异常**，然后正常处理
   - 注：recover是一个内置函数，可以捕获异常



1. 入门案例：

   - ```go
     func test() {
     	
         // 此处使用defer+recover来捕获和处理异常
     	defer func() {
             // recover()内置函数，可以捕获到异常
     		err := recover()
     		if err != nil {
     			fmt.Println(err)
     		}
     	}()
     
     	n1 := 10
     	n2 := 0
     	res := n1 / n2
     	fmt.Println(res)
     
     ```

   - 疑问：1. 不用defer，直接把匿名函数语句写在函数最后，失败；2. 直接res := 10/0 失败



### 2. 自定义错误

 1. 使用`errors.New`和`panic`内置函数自定义错误

 2. `errors.New("错误说明")`，会返回一个error类型的值，表示一个错误

 3. `panic`内置函数，接受一个`interface{}类型的值`(也就是任何值)作为参数。可以接受error类型的变量，**输出错误信息，并退出程序**

 4. 入门案例

     1. ```go
        // 函数读取配置文件信息
        // 如果文件名传入不正确，返回自定义错误
        func readConf(name string) (err error) {
        	if name == "config.ini" {
        		// 读取。。。
        		return nil
        	}else {
        		return errors.New("文件读取错误")
        	}
        }
        func test(fileName string) {
        
        	err := readConf(fileName)
        	if err != nil {
        		// 如果读取文件错误，就输出错误，并终止程序
        		panic(err)
        	}
        
        	fmt.Println("程序继续")
        }
        ```

        

 # 6. 数组和切片

## 1. 数组

### 1. 基本介绍



1. **数组**：可以存放**多个同一类型数据**。也是一种数据类型，在Go中是值类型

2. **语法**：`var 数字名 [数字大小]数据类型`
   
   - `var a [5]int`
   
3. **数组的地址**，就是数组第一个元素的地址
   
   - `&a == &a[0]`
- 数组各个元素的地址间隔是依据数组的类型决定，比如int64->8, int32->4
4. **四种初始化数组的方式**

   - 数组字面值：

     - ```go
       var q [3]int = [3]int{1, 2, 3}
       var r [3]int = [3]int{1, 2}
       fmt.Println(r[2]) // "0"
       ```

   - 类型推导+简化：

     - ```go
       q := [...]int{1, 2, 3}
       fmt.Printf("%T\n", q) // "[3]int"
       ```

   - 指定索引和对应值：

     - ```go
       type Currency int
       const (
           USD Currency = iota // 美元
           EUR // 欧元
           GBP // 英镑
           RMB // 人民币
       )
       symbol := [...]string{USD: "$", EUR: "€", GBP: "￡", RMB: "￥"}
       fmt.Println(RMB, symbol[RMB]) // "3 ￥"
       ```

5. **for-range遍历**：

   1. ```go
      for index, value := range arr{
          /...
      }
      ```

      - index是数组下标，value是该下标位置的值，二者都是for循环内部的局部变量（名称不固定）
      - 如果不想使用下标index，可以直接用`_`替代index

### 2. 注意事项

1. 数组是多个**相同类型数**据的组合，一旦声明，其**长度固定，不能动态变化**
2. `var arr []int`，此时arr就是一个slice切片，
   - 注：中括号里面为空
3. 数组中元素可以是任何数据类型，包括值类型和引用类型
4. 数字创建好后，如果没有赋值，有默认值
5. 使用数组步骤
   - 声明数组并开辟空间
   - 赋值
   - 使用
6. Go的数字属于**值类型**，默认情况下值传递，因此会值拷贝。数组间不会相互影响。
   - 这种机制，导致大数组作为函数参数时，效率很低
   - 传入数组指针效率更高
7. 长度是数组类型的一部分
   1. 写函数时，数字变量括号内的数字要标记清楚
   2. 因为Go认为[3]int和[4]int不是一个数据类型
   3. 长度必须是常量表达式，因为数组的长度需要在编译时确定。
8. 如果想在其他函数中，修改原来的数组，可以使用引用传递（指针）

9. 如果数组的元素类型是可以比较的，那么数组类型也是可以比较的
   - 两个数组所有元素都相等，数组相等
10. 数组是"僵化的"的类型，因为包含了僵化的长度信息，因此作为函数参数时，一般使用slice来代替数组
11. 

### 3. 二维数组

1. 语法：`var 数组名 [大小][大小]类型`

   - `var arr [2][3]int`

2. 内存布局

   - 实际上，二维数组在内存中，只记录每个行数组的地址

   - 也就是说，只记录每行第一个元素的地址
   - ![image-20210501220254136](D:\Desktop\Markdown\Golang\二维数组内存.png)

3. 四种使用方式
   1. `var 数组名 [大小][大小]类型 = [大小][大小]类型{{初值},{初值}}... `
   2. `var 数组名 [大小][大小]类型 = [...][大小]类型{{初值},{初值}}... `
   3. `var 数组名  = [大小][大小]类型{{初值},{初值}}... `
   4. `var 数组名  = [...][大小]类型{{初值},{初值}}... `

4. 遍历
   1. 双层for循环
   2. for-range

## 2. 切片

### 1. 基本介绍

1. 切片是数组的一个引用，因此切片是**引用类型**

2. 使用与数组类似，遍历，访问元素，求元素长度都一样

3. 切片的长度可以变化，因此是一个可以**动态变化的数组**

4. 基本语法：`var 变量名 []类型`

   1. `var a []int`

5. 简单案例

   - ```go
     var arr [5]int = [...]int{1,22,33,66,99}
     
     // 声明一个切片
     // arr[1:3]表示slice引用到arr这个数组index，1-2的元素
     slice := int[1:3]
     ```

### 2. 底层结构

<img src="D:\Desktop\Markdown\Golang\切片的内存布局.png" alt="image-20210430233532316" style="zoom:67%;" />

1. 可以看出slice是引用类型

2. slice从底层来说，其实就是一个struct结构体

   - ```go
     type slice struct {
         prt *[2]int
         len int
         cap int
     }
     ```

   - 由三个部分构成：
     - 指针
     - 长度
     - 容量
   - len：slice中元素数目
   - **cap**：**slice的开始位置到底层数组的结尾长度**
     
     - 也等于：len(slice)-slice起始处索引

### 3. 三种使用方式和遍历

1. 定义一个切片，让切片去引用一个已经创建好的数组。上面的案例就是这样的。

2. 通过make创建

   - `var 切片名 []type = make([]type, len, [cap])`
     - cap：切片容量，可选；
       - 如果选了
         - 要求>=len
         - 相当于是`make([]type,cap)[:len]`
         - 只引用底层数组前len个元素，但容量包含整个数组
       - 不选
         - 默认为len
         - slice是整个数组的view
     - 通过make创建，可以指定切片的大小和容量

3. 定义切片时，直接指定具体元素/数组，原理类似make的方式

   - ```go
     var slice []int = []int {1,3,5}
     ```

   - 注：也可以通过索引和元素值指定，或者混合

---

​	面试题：

​	方式1和方式2的区别：

- 方式1直接引用一个事先存在的数组，数组程序员可见
- 方式2通过make，make也会创建一个数组，由切片在底层维护，程序员不可见

---

1. 具体的遍历方式和数组完全一样

   - 常规

   - for-range

### 4. 注意事项

1. **cap是一个内置函数**，用于统计切片的容量，即最大可以存放多少个元素

2. **切片定义完后，还不能使用**，因为它本身是空的；需要让它引用到一个数组，或者make一个空间

3. **切片可以继续切片**

   - 也就是说多个slice可以共享底层的数据，并且引用的数组部分区间，可能**重叠**

   - ```go
     var arr [5]int = [...]int{1,2,3,4,5}
     slice := arr[:]
     slice2 := slice[1:2]
     
     // slice和slice2都是引用同一个数组
     ```

4. **对于切片的切片：**

   1. 切片长度超出cap将导致一个panic

   2. 如果超出len则意味着扩展了新slice

   3. 案例：

      - ```go
        months := []int{0, 1,2,3,4,5,6,7,8,9,10,11,12}
        summer := months[6:9] // len = 3, cap = len(months)-6=7
        fmt.Println(summer[:20]) // panic:out of range
        endlessSummer := summer[:5] // right
        fmt.Println(endlessSummer)
        ```

5. 因为是引用类型，因此像**函数传递slice可以在函数内部修改底层数组的元素**
   - 假设reverse函数是反转切片，那么一种将**slice元素循环向左旋转n个元素**的方法是，三次调用reverse函数
     - 反转开头的n个元素
     - 反转剩下的元素
     - 反转整个slice
     - 注：如果是右旋转，则第3个步骤放到第1个
6. **与数组不同的是，slice不能进行比较**
   - bytes.Equals可以判断两个字节型slice是否相等，其他类型只能一个个比较元素（先比较len是否相等）
   - 原因：
     - slice元素间接引用，甚至可以引用自身
     - 因为是间接引用，一个固定值的slice，在不同的时间可能包含不同的元素
   - 唯一合法的比较操作是和nil比较
     - nil值的slice没底层数组，len和cap都是0
     - 也有非nil值的slice，len和cap都是0的
       - 例如：[]int{}或make([]int, 3)[3:]
       - 上面后者返回[]，
       - 如果是[2:]，返回[0]
   - **注意**：如果要测试一个slice是否为空，使用len(s)==0判断，而不是s==nil判断
     - 因为不为nil的时候，也可能是空的

### 5. append函数

1. 功能：动态追加切片的元素，添加到末尾

2. 底层原理分析
   1. 本质是对数组扩容
   2. 底层会创建一个新的数组newArr（扩容后的）
   3. 将slice原来的元素拷贝到newArr
   4. slice重新引用newArr
   5. 注意newArr是底层维护的

3. 入门案例：

   - 追加具体元素，可多个

     - ```go
       var slice []int = []int{1,2,3}
       
       // append会生成新切片，因此要再赋给slice
       slice = append(slice, 4,5,6)
       
       // 如果像下面这样，没有上面的代码，slice本身不会发生变化
       slice2 = append(slice, 4,5,6)
       ```

   - 追加切片

     - ```go
       slice = append(slice, slice...)
       //...表示接收变长参数的slice
       ```

### 6. copy函数

1. 语法：`copy(dst,src)`

   - ```go
     var a []int = []int{1,2,3,4,5}
     
     var slice = make([]int,10)
     
     copy(slice,a) //把a的元素copy到slice中
     ```

   - copy的两个参数都是切片类型

   - 返回成功复制元素的个数，等于两个slice中较小的长度

2. 注意：

   - slice和a的数据空间是独立的，相互不影响
   - 即便silce大小没有a大，copy也会成功
     - 只会copy给len(slice)个数据给slice
   - 因为无法确定，原先slice上的操作，是否会影响到新的slice，通常将append的结果直接赋值给目标扩展的slice变量

### 7. string和slice

1. string底层是一个byte数组，因此可以进行切片处理

   - ```go
     var s = "abdfafa"
     slice := s[:]
     ```

   - 注：上面得到的slice，**实际上的数据类型也是string**

   - **string的底层，本质上也是一个切片**

2. string是不可变的

3. 如果需要修改字符串，

   1. 可以先将string -> []byte 或 ->rune 

   2. 修改切片

   3. 重写转成string

   4. 注：**[]byte按字节处理，因此无法处理中文**；如果字符串中有中文需要处理，则转成[]rune切片，**[]rune按照字符处理，兼容汉字**

      ```go
      str := "abcads"
      
      slice := []byte(str)
      slice[0] = 'z'
      str = string(slice)
      ```

### 8. 内存小技巧

1. 可以模拟一个stack

   - ```go
     stack = append(stack, v) // push v
     top := stack[len(stack)-1] // top of stack
     stack = stack[:len(stack)-1] // pop
     ```

2. 要删除slice中间的某个元素并保存原有的元素顺序，可以通过内置的copy函数将后面的子slice向前依次移动一位完成：

   - ```go
     func remove(slice []int, i int) []int {
         copy(slice[i:], slice[i+1:])
         return slice[:len(slice)-1]
     }
     func main() {
         s := []int{5, 6, 7, 8, 9}
         fmt.Println(remove(s, 2)) // "[5 6 8 9]"
     }
     ```

3. 如果删除元素后不用保持原来顺序的话，我们可以简单的用最后一个元素覆盖被删除的元素

   - ```go
     func remove(slice []int, i int) []int {
         slice[i] = slice[len(slice)-1]
         return slice[:len(slice)-1]
     }
     func main() {
         s := []int{5, 6, 7, 8, 9}
         fmt.Println(remove(s, 2)) // "[5 6 9 8]
     }
     ```

     

# 7. Map

## 1. 基本语法

1. `var 变量名 map[keytype]valuetype`
   1. key的数据类型：

      - 可以是很多种数据类型，比如bool，数字，string，指针，channel；还可以是包含前面几个类型的接口，结构体，数组（较少）

      - **通常为int，string**
      - 注意：slice，map，还有function不可以，因为他们没法用`==`来判断

   2. value的数据类型

      - 基本和key一样
      - 通常为数字，string，map，struct

   3. 注意：声明map时不会分配内存的，初始化需要make，分配内存之后才能赋值和使用。

      - 因此map使用前一定要make（除非直接赋值）
      - make用法：`make(map[类型][类型], size)`
        - size是初始分配的大小，可忽略

## 2. map的使用方式(类型推导也可以)

1. 先声明，再make

   ```go
   // 此时map1=nil
   var map1 map[string]string
   
   map1 = make(map[string]string,10)
   ```

2. 声明时，直接make

   ```go
   var map1 = make(map[string]string)
   ```

3. 声明时，直接赋值

   ```go
   var map1 map[string]string = map[string]string{
       "no4":"成都", //注意，map最后一个key-value结尾必须有逗号
   }
   ```

## 3. map的curd操作

1. 增加和更新
   
   - `map[key] = value`：如果key没有，就是增加；如果已有，就是修改
   
2. 删除
   - `delete(map,key)`
     - delete是一个内置函数，若key存在，就删除键值对；若不存在，不操作，也不报错
   - 注意事项
     - 如果要删除map的所有key，没有专门的方法一次删除，比如其他语言的clean
     - 可以遍历所有key，逐个删除
     - 或者`map = make(...)`，make一个新的，使得原先的被gc回收（推荐）
   
3. 查找：`val, ok := map[key]`

   - 如果ok为true，则存在key；若为false，则不存在

   - 或者，下面这样也是可以的

     - ```go
       if map[key] != nil {
           
       }
       ```

4. map遍历:使用for-range

   - 

     ```go
     for k,v := range map{
         
     }
     ```

5. map长度：len(map)



## 4. map的切片

1. 切片的数据类型如果是map，称为slice of map，map切片。这样map的个数可以动态变化

2. 案例：

   - 要求：使用一个map记录monster的信息name和age。也就是说一个monster对应一个map，并且monster的个数可以动态增加

   - ```go
     // 1. 声明一个map切片, 并且分配空间
     var monsters []map[string]string
     monsters = make([]map[string]string, 2)
     
     // 2. 添加第一个妖怪信息
     if monsters[0] == nil{
         monsters[0] = make(map[string]string,2)
         monsters[0]["name"] = "niumowang"
         monsters["age"] = "500"
     }
     
     if monsters[1] == nil{
         monsters[1] = make(map[string]string,2)
         monsters[1]["name"] = "yutujing"
         monsters["age"] = "200"
     }
     
     // 3. 下面要使用切片的append函数，动态添加map
     
     newMonster := map[string]string{
         "name":"xinyaoguai"
         "age":"20"
     }
     
     monsters = append(monsters, newMonster)
     ```

## 5. map排序

1. 基本介绍

   - Go中没有一个专门的方法对map的key排序，（好新版本是升序了？）

2. 排序步骤

   1. 先将map的key放到切片中

   2. 对切片排序

   3. 遍历切片，然后按照key输出value

   4. ```go
      newMap := make(map[int]int)
      newMap[1] = 2
      newMap[10]=200
      newMap[50]=42
      newMap[-3] = 9
      
      var keys []int
      for k,_ := range newMap{
          keys = append(keys, k)
      }
      
      // 排序
      sort.Ints(key)
      ```

   

## 6. 使用细节

1. map是**引用类型**
2. map容量达到后，继续像map添加元素，会自动扩容；也就说map可以**动态的增长键值对**
3. map的value也经常使用struct类型，更是个管理复杂的数据（比之前的value是一个map要更好）



# 8. Go的面向对象编程

1. **Golang面向对象编程说明**
   1. Golang虽然支持面向对象，但是和传统的面向对象编程(OOP)有区别，不是纯粹的面向对象语言
      - 因此更准去的是说，Golang**支持面向对象编程特性**
   2. Golang没有类，Go的结构体struct和其他编程语言的类有同等的地位，可以理解为Golang是基于struct来实现OOP特性的
      - 可以理解为：struct相比于类，它**只能描述属性，但不能描述行为**
   3. Go的面向对象非常简洁，去掉了传统OOP的继承，方法重载，构造函数，析构函数，隐藏的this指针等
   4. Go仍然有面向对象编程的**继承，封装，多态的特性**，只是实现方式和其他OOP语言不一样。**比如继承：Golang没有extends关键字，继承通过匿名字段实现**
   5. Go的OOP很优雅，OOP本身就是语言类型系统的一部分，通过接口（interface）关联，耦合性低，非常灵活。
      - 也就是说，Golang中，**面向接口编程**是非常重要的特定。

## 1. 结构体

### 1. 基本介绍

1. 结构体和结构体变量（实例）的区别和联系
   1. 结构体是自定义的数据类型，类似于其他语言类的概念；不过只描述属性，不描述行为
   2. 结构体变量（实例）是具体的，实际的，代表具体变量
2. 结构体变量(实例)在内存的布局

![image-20210502205958148](D:\Desktop\Markdown\Golang\结构体变量内存.png)



3. 声明：

   - ```go
     type 结构体名称 struct{
         field1 type
         field2 type
     }
     
     // 举例
     typpe Student struct{
         Name string
         Age int
         Score float32
     }
     ```

   - 结构体名称和内部的字段/属性，是指针，

4. 字段/属性：结构体字段=属性=filed

   - 是结构体的一个组成部分，一般是**基本数据类型、数组，也可以是引用类型**
   - 注意事项：
     - 字段声明语法同变量：字段名 字段类型
     - **不同结构体变量的字段是独立的**
       - 结构体是**值类型**
     - 注意：如果字段slice，map的话，要先make才能使用；是指针的话要先赋地址

 

### 2. 创建结构体实例的四种方式

```go
type Person struct {
	Name string
	age int
}
```

1. **直接声明**：
   - `var person Person`

2. **{}**——常用，推荐
   - `var person Person = Person{"mary",18} `
     - 注：可以在结构体内按照字段定义顺序，直接给字段赋值（可选）
     - 也可以按照键值对的方式在定义时赋值哦，就不依赖字段的定义顺序了
3. **&**
   - `var person *Person = new (Person)`
   - 理论上赋值应该是：`(*person).Name` = "aaa"；但其实在go中，直接`person.Name="aaa"`就可以
     - 编译器底层会自动加上取值的`*`的操作
4. **第二种{}**
   - `var person *Person = &Person{}`
     - 也可以直接在括号里面赋值
5. **说明**：
   1. 上面的四种方式，都可以使用**类型推导**
   2. 第3、4中方式返回的是**结构体指针**
      - **结构体指针.字段名**是Go做的优化，符合使用习惯

### 3. 使用细节

1. 结构体的所有字段，在内存中是连续的

   - 案例：

     ```go
     type Point struct{
         x,y int
     }
     
     type Rect struct{
         leftUp,rightDown Point
     }
     
     func main(){
         r1 := Rect{Point{1,2}, Point{3,4}}
         // r1有4个int，在内存中都是连续分布
     
     }
     ```

   - 如果上面Rect结构体的字段是`*Point`类型的，那么两个`*Point`类型本身的地址也是连续的

     - 它们指向的地址不一定连续

2. 结构体是用户单独定义的类型，和其他类型进行转换时，**需要有完全相同的字段（名字，个数和类型）**

   - 两个结构体互相转换时

3. 结构体可以被type重新定义，相当于取别名；go认为是新的数据类型，但是二者可以相互强转

   - ```go
     type Student struct{
         Name string
         Age int
     }
     
     type Stu Student
     
     func main(){
         var stu1 Student
         var stu2 Stu
         stu2 = stu1 // 错误
         stu2 = Stu(stu1) // 正确
     }
     ```



4. struct的每个字段上，可以写上一个tag，这个tag可以通过反射机制获取；常见的常见就是序列化和反序列化

   - 场景：比如说要把一个对象传给浏览器，那么要传回对象的json形式，需要使用到`json.Marshal(对象)`这个方法

   - 但是由于浏览器常希望json里面的字段首字母是小写的，`json.Marshal(对象)`处理对象时，相当于从其他包使用这个结构体，因此结构体的字段名称，如果设置为小写，就不能再其他包访问

   - 因此要设置tag标签

   - 案例

     ```go
     type Monster struct{
         Name string `json: "name"`
         Age int `json:"age"`
     }
     
     func main(){
         monster := Monster{"niumowang",500}
         
         jsonStr, err := json.Marshal(monster)
         if err != nil {
             fmt.Println(err)
         }
         
         fmt.Println("jsonStr", string(jsonStr))
     }
     ```

     

## 2. 方法

### 1. 基本介绍



1. 介绍：

   1. 结构体除了有一些字段外，还需要有一些行为，这就需要方法来完成
   2. Golang的**方法是作用在指定的数据类型上**的（即：和指定的数据类型绑定），因此**自定义类型，都可以有方法**，不仅仅是struct

2. 声明和调用

   1. 声明

      - ```go
        func (recevier type) methodName (参数列表) (返回值列表){
            方法体
            return 返回值
        }
        ```

      - 参数列表：表示方法输入

      - recevier type：

        - 方式这个方法和type这个类型进行绑定，或者说该方法作用于type类型
        - type可以使结构体，也可以是其他自定义类型
        - recevier：就是type类型的一个变量（实例）
        - return并不是必须的

   2. 举例

   ```go
   type A struct{
       Num int
   }
   
   func (a A) test(){ // test只能被A数据类型的变量调用
       	// 也可以说结构体A有一个方法test
       
   }
   
   func main() {
       var t A
       t.test()
   }
   ```

   - 说明：
     - func **(a A)** test() 表示，A结构体有一个方法，叫test

3. 入门案例：

   - ```go
     type Person struct{
         Name string
     }
     
     func (p Person) test(){
         fmt.Println("test() name=", p.Name)
     }
     
     func main(){
         var p Person
         p.Name = "tom"
     }
     ```

   - 总结：

     - test方法和Person类型绑定
     - test方法只能通过Person类型的变量来调用
       - 不能直接调用，也不能用其他类型变量来调用
     - 传参的机制，和函数是一样的。每次Person变量调用test()，只是穿进去一个变量的副本，并且结构体变量是值拷贝
     - **注意：**方法也是可以接受参数的，并且有返回值的
     - 个人理解：**方法在形式上，就是在func关键字和函数名之间，添加了(结构体变量 结构体类型)**



### 2. 方法的调用和传参机制

- 方法的调用和传参机制和函数基本一样。区别在于，方法调用时，**调用方法的变量也会当做实参传递给方法。**
  - 如果变量是值类型，进行值拷贝
  - 变量是引用类型，则进行地址拷贝



### 3. 注意事项

1. 结构体类型是值类型，方法调用中，是值拷贝的传递方式

2. 如果希望在方法中，修改结构体变量的值，可以通过结构体指针的方式

   - 注意：和前面提到的编译器优化相同，如果方法是和**结构体指针绑定**的，在调用方法时，不需要用结构体变量的地址去调用，**直接用结构体变量调用**即可

3. Golang中的方法作用在指定的数据类型上

   - 即，和指定的数据类型绑定

4. 方法的访问范围的控制规则，和函数一样，通过首字母大小写

5. 如果一个变量实现了`String()`方法，那么`fmt.Println`会默认调用这个变量的`String()`进行输出

   

### 4. 和函数的区别

1. 调用方式不同

   - 函数：函数名(实参列表)
   - 方法：变量.方法名(实参列表)

2. 对于普通函数，接受者为值类型时，不能将指针类型的数据直接传递，反之亦然

3. 对于方法，例如struct的方法，接受者为值类型时，可以直接用指针类型的变量调用方法，反之同理

   -  ```go
     type Person struct{
         Name string
     }
     
     func (p Person) test(){
         //
     }
     
     func main(){
         p := Person{"tom"}
         
         p.test()
         
         // 两种调用都可以
         // 并且下面只是形式上传了地址，实际上依旧是值拷贝的
         (&p).test()
         
         // 而上面那种调用，函数是不允许的
     }
     ```

   - 方法的参数，是值拷贝还是地址拷贝，**只取决于它绑定的变量是值类型，还是指针**。



## 3. 工厂模式

​	Golang结构体没有构造函数，通常可以使用工厂模式来解决问题

1. 需求

![image-20210503232625355](C:\Users\lunatic\AppData\Roaming\Typora\typora-user-images\image-20210503232625355.png)

2. 工厂模式解决

- ```go
  package model
  
  // 结构体名小写，其他包不能使用
  type student struct{
      Name string
      
      // 字段首字母小写，其他包不能使用
      score float64
  }
  
  // 类似于构造函数，返回一个student变量
  func NewStudent(n string, s float64) *student{
      return &student{n,s}
  }
  
  // 提供一个方法，访问其他包不能使用的变量
  func (s *student) GetScore() float64{
      return s.score
  }
  ```

  

- ```go
  package main
  import (
      "model"
      "fmt"
  )
  
  func main(){
      // 利用model模块的其他函数，来获得该结构体变量
      stu := model.NewStudent("tom","99.5")
      fmt.Println(stu.Name, stu.GetScore())
  }
  ```

  

## 4. Golang面向对象编程三特性

### 1. 封装

1. 介绍：封装就是把抽象出的**字段和对字段的操作**，封装在一起，数据被保护在内部；程序的其他包，只有通过被授权的操作（方法），才能对字段进行操作

2. 好处：

   - 隐藏实现细节
   - 可以对数据进行验证，保证安全合理

3. 体现：

   - 对结构体的属性封装（比如小写）
   - 通过方法，包才能访问被封装的属性、方法 

4. **实现步骤：**

   1. 将结构体，字段的首字母小写

   2. 给结构体所在包提供一个工厂模式的函数，首字母大写，类似构造函数

   3. 提供一个首字母大写的Set方法，对属性判断，并复制

      ```go
      func (var 结构体名) SetXXX(参数列表)(返回值列表){
          // 数据验证逻辑
          var.字段 = 参数
      }
      ```

      

   4. 提供一个首字母大写的Get方法，用户获取属性的值

   5. **特别说明：**Golang没有特别强调封装，不像Java，Golang本身对面向对象的特性做了简化 



### 2. 继承

#### 1. 基本介绍

1. 解决代码复用

   Golang中，如果一个struct嵌套了另一个匿名结构体，那么这个结构体可以直接访问匿名结构体的字段和方法，从而实现继承特性

2. 好处

   1. 代码复用性提高
   2. 扩展性和维护性也提高了

3. **基本语法**

   ```go
   type Goods struct{
       Name string
       Price int
   }
   
   type Book struct{
       Goods // 这里就是嵌套匿名结构体
       Writer string
   }
   ```

4. 入门案例

   - ```go
     // 父结构体
     type Student struct{
         Name string
         Age int
         Score int
     }
     
     func (stu *Student) ShowInfo(){
         fmt.Printf("...")
     }
     
     func (stu *Student) SetScore(score int){
         // 逻辑判断
         stu.score = score
     }
     
     
     // 继承
     type Pupil struct{
         Student // 嵌入了Student匿名结构体，表继承
     }
     
     // Pupil特有方法
     func (pupil *Pupil) test(){
         fmt.Println()
     }
     
     func main(){
         
         // 使用
         pupil := &Pupil{}
         
         // 后面还有更简洁的方式，这里的比较容易理解
         pupil.student.Name = "tom"
         pupil.student.Age = 8
         pupil.test()
         pupil.student.SetScore(70)
         pupil.student.ShowInfo()
     }
     ```

#### 2. 深入讨论

1. 结构体可以使用嵌套匿名结构体**所有的字段和方法**，也就是说，不管首字母大小写，都可以使用

2. 匿名结构体的字段和方法的访问，**可以简化**

   - 例如上面的案例中，可以按照下面写

   ```go
   pupil.Name = "tom"
   pupil.Age = 8
   pupil.test()
   pupil.SetScore(70)
   pupil.ShowInfo()
   ```

3. 当结构体和匿名结构体有相同的字段和方法时，编译器采用**就近访问原则**。如果希望访问匿名结构体的字段和方法，可以通过**匿名结构体名**来区分（像入门案例里面写的方式）

   1. 方法优先使用自己的字段，没有的话再去上一级

   2. ```go
      type A struct{
      	Name string
      	age int
      }
      
      func (a *A) SayOk(){
      	fmt.Println("Ok", a.Name)
      }
      
      func (a *A) hello(){
      	fmt.Println("hello ", a.Name)
      }
      
      type B struct{
      	A
          Name string
      }
      
      func (b *B) SayOk(){
      	fmt.Println("Ok", b.Name)
      }
      
      func main(){
          var b B
          b.Name = "jack"
          b.age = 100
          b.SayOK() // 输出 "Ok jack"
          
          b.hello() // 输出 "hello "
          // 注意，因为B结构体没有hello方法，因此调用的是A结构体的hello方法，而A的hello方法中使用的是a.Name，没有赋值，因此是一个空串
          // A的方法，首先用A自己的字段
          
          // 可以这样给A结构体的name赋值
          b.A.Name = "mary"
      }
      ```

   - 小结：
     - 通过b访问字段或方法时，比如说b.Name，**流程**如下
     - 编译器先看b对应的类型有没有Name，如果有，直接调用B类型的Name字段
     - 如果没有，就去看B中嵌入的匿名结构体A有没有声明A，如果有就调用，没有继续向上查找，都找不到就报错

4. 结构体嵌入两个或多个匿名结构体，若**两个匿名结构体有相同的字段或方法**（**同时该结构体本身没有同名的字段和方法**），在访问时，就必须明确指定匿名结构体的名字，否则编译报错

5. 如果一个struct嵌套了一个**有名结构体**，这种模式就是**组合**；如果是组合关系，那么访问组合的结构体的字段或方法时，必须带上结构体的名字

   - ```go
     type A struct{
         Name string
         age int
     }
     
     type B struct{
         a A // 有名结构体
     }
     
     func main(){
         var d D
         d.Name = "jack" // 错误
         d.a.Name = "jack" // 正确
     }
     ```

6. 嵌套匿名结构体后，也可以在创建结构体变量时，直接指定各个匿名结构体字段的值

   - 举例

   - ```go
     type Goods struct{
     	Name string
     	Price float64
     }
     
     type Brand struct {
     	Name string
     	Address string
     }
     
     type TV struct {
     	Goods
     	*Brand
     }
     
     func main() {
     	tv := TV{ 
         Goods{"电视剧001",5000.88},
     	&Brand{"海尔", "山东"},}
     	fmt.Println(tv)
         // 输出的话， brand部分为地址
         // 可以用*tv.Brand看到Brand部分
     }
     ```

#### 3. 多重继承

	1. 概念：一个struct嵌套多个匿名结构体
	2. 如果嵌入的匿名结构体，有相同的字段或方法名，同时该结构体本身没有同名的字段和方法，那么访问时，需要通过匿名结构体类型名来区分
	3. 为了保证代码简洁性，建议尽量不用



### 3. 多态

1. 基本介绍：变量具有多种形态；在Golang中，**多态是通过接口实现的**。可以按照统一的接口，来调用不同的实现，这样接口变量就呈现不同的形态
2. 接口体现多态：
   1. 多态参数
      - 前面的usb接口，既可以接受手机变量，又可以接受相机变量，体现了Usb接口的多态
   2. 多态数组
      - 案例：一个Usb数组，存放phone结构体和camera结构体变量。phone有一个特有方法call。遍历数组，若为phone变量，除了调用usb方法外，还调用call
        - 使用类型断言即可



## 5. 接口

### 1. 基本介绍

1. interface类型可以定义一组方法，但是**不需要实现**。并且interface**不能包含任何变量**。到某个自定义类型（比如结构体）要用的时候，在根据具体情况把这些方法写出来。

2. 基本语法：

   - ```go
     type 接口名 interface{
         method1(参数列表) (返回值列表)
         method2(参数列表) (返回值列表)
     }
     
     // 实现接口所有方法
     func (t 自定义类型) method1(参数列表)(返回值列表){
         // 方法实现
     }
     
     func (t 自定义类型) method2(参数列表)(返回值列表){
         // 方法实现
     }
     ```

   - 小结：

     - 接口里所有方法都没方法体，体现了程序设计的**多态和高内聚低耦合**的思想
     - Golang中的接口，**不需要显示的实现**。**只要有一个变量，含有接口类型中的<u>所有方法</u>，那么这个变量就实现了这个接口**。因此Golang中，没有implement这样的关键字。

2. 应用场景
   1. 比如，国家要制造飞机，专家只需要把飞机需要的功能/规格定下来即可，然后让别的人实现
   2. 项目经理管理程序员，为了控制和管理软件，可以定义一些接口，由程序员具体实现

### 2. 注意事项和细节

1. 接口本身不能创建实例，但是可以指向一个实现了该接口的自定义类型的变量（实例）

   - ```go
     var stu Stu // 结构体变量，实现了接口Ainterface
     var a Ainterface = stu // 可以指向
     ```

2. 接口中的所有方法，都没有方法体，都是未实现的方法

3. Golang中，一个自定义类型，需要将某个接口的所有方法都实现，才能说它实现了这个接口

4. 一个自定义接口，只有实现了某个接口，才能将该自定义类型的实例（变量）赋给接口类型

5. 只要是自定义数据类型，都可以实现接口，不一定非要是结构体

6. 一个自定义类型可以实现多个接口

7. Golang接口中，不能有任何变量

8. 一个接口（A）可以**继承多个**别的接口（B,C)，这是如果要实现A接口，也必须将B,C接口的方法全部实现

9. **interface类型默认是指针（引用类型）**，若没有对interface初始化就使用，那么会输出nil

10. 空接口interface{}没有任何方法，所以所有类型都实现了空接口。即我们可以把任何一个变量赋值给空接口类型

11. 案例：

    <img src="D:\Desktop\Markdown\Golang\接口案例.png" alt="image-20210505003008670" style="zoom:50%;" />



### 3. 最佳实践

实现对Hero结构体切片的排序：sort.Sort(data Interface)

```go
package main

import (
	"fmt"
	"math/rand"
	"sort"
)

type Hero struct {
	Name string
	Age int
}

// 这是啥，别名吗
type HeroSlice []Hero

func (h HeroSlice) Len() int  {
	return len(h)
}

// less方法决定使用什么标准排序
// 此处按照Hero年龄升序排序
func (h HeroSlice) Less(i,j int) bool{
	return h[i].Age < h[j].Age
}

func (h HeroSlice) Swap(i,j int) {
	temp := h[i]
	h[i] = h[j]
	h[j] = temp
}
func main() {
	var heroes HeroSlice

	// 初始化
	for i := 0; i < 10; i++ {
		hero := Hero{
			Name : fmt.Sprintf("英雄~%d", rand.Intn(100)),
			Age : rand.Intn(100),
		}
		heroes = append(heroes, hero)
	}

	// 排序前的顺序
	for _,v := range heroes {
		fmt.Println(v)
	}

	sort.Sort(heroes)
	fmt.Println()
	for _,v := range heroes {
		fmt.Println(v)
	}
}

```



### 4. 实现接口和继承比较

1. 当结构体需要扩展功能，又不喜欢破坏继承关系，则可以通过实现某个接口
   - 因此在这里，可以认为实现接口是对继承机制的一种补充
2. 解决的问题不同：
   - 继承的价值：解决代码的**复用性和可维护性**
   - 接口的价值：**设计**好各种规范（方法），让其他自定义类型去实现这些方法
3. 接口比继承更加灵活
   - 继承：is-a的关系
   - 接口：like-a的关系
4. 接口一定程度上实现代码解耦



## 6. 类型断言

1. **基本介绍**：类型断言，由于接口是一般类型，不知道接口变量的具体类型是什么，如果要转成具体类型，就需要使用类型断言

   - ```go
     var t float64
     var x interface{}
     x = t
     var y float64
     y = x // erroe
     
     // 类型断言，判断x是否是指向float64的变量
     // 如果不成功会报错
     y = x.(float64)
     
     // 类型断言还可以返回err，查看是否成功
     // 这样就不会报错了
     y, ok = x.(float64)
     ```

   - 注：断言是最好带上**检测机制**

2. 经典案例：

   - ```go
     // 函数，判断输入的参数是什么类型
     func typeJudge(items ...interface{}){
     	for index, x := range items{
     		switch x.(type) {
     		case bool:
     			fmt.Printf("第%v个参数是 bool类型，值是%v", index, x)
     		case string:
     			fmt.Printf("第%v个参数是 string类型，值是%v", index, x)
     		case int, int64:
     			fmt.Printf("第%v个参数是 int类型，值是%v", index, x)
     		// 。。。
     		}
     	}
     }
     
     func main(){
         var n1 float32 = 1.1
         var n2 float64 = 2.3
         var n3 int32 = 30
         var name string = 1.1
         address := "beij"
         n4 := 300
         typeJudge(n1,n2,n3,name,address)
         
     }
     ```

   - 注：`x.(type)`只能和switch搭配，判断变量的具体类型



# 9. 文件操作

## 1. 基本介绍

1. 文件在程序中是以流的形式来操作的

- **流**：数据在数据源（文件）和程序（内存）之间经历的路径
- **输入流**：数据从数据源（文件）到程序（内存）的路径
- **输出流**：数据从程序（内存）到数据源（文件）的路径

2. **os包的File结构体**封装了所有文件相关的操作



## 2. 常用的文件操作函数和方法

1. **打开一个文件**，进行读操作

   - `os.Open(name string)(*File, error)`
   - 返回一个**文件指针**

2. **关闭一个文件** 

   - `File.close()`

3. **读取文件操作应用实例**

   1. 读取文件内容，并且显示在终端（带**缓冲区**的方式）；缓冲区适合大文件

      - ```go
        file , err := os.Open("a.txt")
        if err != nil {
            fmt.Println(err)
        }
        
        // 要及时关闭file句柄，否则会有内存泄漏
        defer file.Close()
        
        // 创建一个 *Reader，带缓冲的
        // 默认缓冲区为4096个字节
        reader := bufio.NewReader(file)
        
        // 循环读取文件内容
        i := 0
        for {
            fmt.Println(i)
            i++
            // 读到一个换行符就结束，相当于按行读取
            str, err := reader.ReadString('\n')
            if err == io.EOF{ // io.EOF表示读到文件末尾
                break
            }
            // 因为本身会把换行符读进去，因此不需要Println
            fmt.Print(str)
        }
        
        fmt.Println("文件读取结束")
        ```

   2. 读取文件的内容并显示在终端（使用**ioutil一次将整个文件读入到内存中**）；这种方法适用于文件不大的情况

      - ```go
        // 返回一个[]byte切片
        content , err := ioutil.ReadFile("a.txt")
        if err != nil {
            fmt.Println(err)
        }
        
        // 因为没有显示的Open文件，因此也不需要显示的close文件
        // 因为文件的open和close被封装到ReadFile里面了
        
        fmt.Printf("%v",string(content))
        ```

4. **写文件操作应用：**

   1. `os.OpenFile(name string, flag int, perm FileMode) (file *file, err error)`

      1. 说明：os.OpenFile是一个个更一般性的文件打开函数，它会使用指定的选项（如O_RDONLY等）、指定的模式（如0666等）打开指定名称的文件。如果操作成功，返回的文件对象可用于I/O；如果出错，错误底层类型是*PathError

      2. 第一个参数：文件名

      3. 第二个参数：文件打开模式（可组合）

         - ```go
           const (
               O_RDONLY int = syscall.O_RDONLY // 只读模式打开文件
               O_WRONLY int = syscall.O_WRONLY // 只写模式打开文件
               O_RDWR   int = syscall.O_RDWR   // 读写模式打开文件
               O_APPEND int = syscall.O_APPEND // 写操作时将数据附加到文件尾部
               O_CREATE int = syscall.O_CREAT  // 如果不存在将创建一个新文件
               O_EXCL   int = syscall.O_EXCL   // 和O_CREATE配合使用，文件必须不存在
               O_SYNC   int = syscall.O_SYNC   // 打开文件用于同步I/O
               O_TRUNC  int = syscall.O_TRUNC  // 如果可能，打开时清空文件
           )
           ```

      4. 第三个参数：权限控制，在linux和unix操作系统下才有效

   2. 案例1：

      - ```go
        	// 创建新文件，并写入内容
          	name := "b.txt"
          	file, err := os.OpenFile(name, os.O_WRONLY | os.O_CREATE,0666)
          	if err != nil {
          		fmt.Println(err)
          		return
          	}
          	// 及时关闭file句柄
          	defer file.Close()
          
          	// 准备写入
          	str := "hello,Gardon\n"
          	// 写入时，使用带缓冲的 *Writer
          	writer := bufio.NewWriter(file)
          
          	for i := 0; i < 5; i++ {
          		writer.WriteString(str)
          	}
          
          	// 因为writer带缓存，因此调用WriteString时，内容先写到缓存上
          	// 需要flush函数将缓存写到磁盘上(文件中）
          	writer.Flush()
        ```

   3. 案例3：

      - ```go
        
        // 将一个个文件的内容，写入到另外一个文件
        func main(){
        	data, err := ioutil.ReadFile(name1)
        	if err != nil {
        		return
        	}
        
        // 一次性写入文件
        
        	err = ioutil.WriteFile(name2, data, 0666)
        	if err != nil {
        		return
        	}
        }
        ---------------------------------
        // 第二种方式
        
        
        func main() {
        	// 创建新文件，并写入内容
        	name2 := "b.txt"
        	name1 := "a.txt"
        	file1, err1 := os.Open(name1)
        	file2, err2 := os.OpenFile(name2, os.O_WRONLY,0666)
        
        	if err1 != nil || err2 != nil {
        		fmt.Println(err1,err2)
        		return
        	}
        
        	defer file1.Close()
        	defer file2.Close()
        
        	reader := bufio.NewReader(file1)
        	writer := bufio.NewWriter(file2)
        
        	for {
        		str, err3 := reader.ReadString('\n')
        		//由于读最后一行时，就返回EOF，而此时break
                //会导致最后一行的str没有写入文件
                //因此write的操作应该在break判断的前面
                writer.WriteString(str)
        		if err3 == io.EOF {
        			break
        		}
                // 在这里的话会无法写入最后一行
                //writer.WriteString(str)	
        	}
        	writer.Flush()
        }
        ```

5. **判断文件是否存在**

   - golang判断文件或文件夹是否存在的方法为使用`os.Stat()`函数返回的错误值进行判断：
     - 如果返回的错误为nil，说明文件或文件夹存在
     - 如果返回的错误类型使用`os.IsNotExist()`判断为true，说明文件或文件夹不存在
     - 如果返回的是其他类型，则不确定
     
   - 可以用这个函数来判断（老师写的）
   
     - ```go
       fucn PathExists(path string) (bool, error){
           _, err := os.Stat(path)
           if err == nil { // 文件或目录存在
               return true, nil
           }
           if os.IsNotExist(err){
               returnn false, nil
           }
           return false,err
       }
       ```
   
     - 

6. **拷贝文件**

   1. 利用io包提供的`func Copy(dst Writer, src Reader) (written int64, err error)`

      - 返回拷贝的字节数和遇到的第一个错误

   2. ```go
      package main
      
      import (
      	"bufio"
      	"fmt"
      	"io"
      	"os"
      )
      
      // 编写函数，接受两个文件路径
      func CopyFile(srcPath string, dstPath string) (written int64, err error) {
      	srcFile, err := os.Open(srcPath)
      	if err != nil {
      		fmt.Println(err)
      		return 0, err
      	}
      	defer srcFile.Close()
      	reader := bufio.NewReader(srcFile)
      
      	dstFile, err := os.OpenFile(dstPath, os.O_WRONLY|os.O_CREATE, 0666)
      	if err != nil {
      		fmt.Println(err)
      		return 0, err
      	}
      	defer dstFile.Close()
      	writer := bufio.NewWriter(dstFile)
      	return io.Copy(writer, reader)
      }
      
      func main() {
      	srcPath := "1.jpg"
      	dstPath := "C:\\Users\\lunatic\\Desktop\\憨憨专用\\2.jpg"
      	_,err := CopyFile(srcPath, dstPath)
      	if err == nil {
      		fmt.Printf("拷贝完成")
      	}else {
      		fmt.Println(err)
      	}
      }
      
      ```

## 3. 文件编程应用实例

1. 统计英文、数字、空格和其他字符的数量

   - ```go
     package main
     
     import (
     	"bufio"
     	"fmt"
     	"io"
     	"os"
     )
     
     // 定义一个个结构体，用户保存统计结果
     type CharCount struct {
     	ChCount int // 英文
     	NumCount int // 数字
     	SpaceCount int // 空格
     	otherCount int // 其他字符
     
     }
     
     func main() {
     	//var a int32 = 1
     	//var b bool = a <= '2'
     	//fmt.Println(b)
     	// 思路：打开文件，创建一个reader
     	// 读取每一行，就统计该行各种字符的数量
     	// 然后将结果保存到一个结构体
     	fileName := "a.txt"
     	file, err := os.Open(fileName)
     	if err != nil {
     		fmt.Println(err)
     		return
     	}
     	defer file.Close()
     
     	var charCount CharCount
     
     	reader := bufio.NewReader(file)
     
     	for {
     		str, err := reader.ReadString('\n')
     		// 为了兼容中文字符，可以将str转成[]rune
     		for _, v := range str {
                 // 注意此处switch后面不能接v，因为case表达式返回布尔类型
                 // 而v是int32，不匹配，编译报错
     			switch {
     				case v >= 'a' && v <= 'z', v >= 'A' && v <= 'Z':
     					charCount.ChCount++
     				case v == ' ' || v == '\t':
     					charCount.SpaceCount++
     				case v >= '0' && v <= '9':
     					charCount.NumCount++
     				default:
     					charCount.otherCount++
     			}
     		}
     		if err == io.EOF{
     			break
     		}
     	}
     
     	fmt.Printf("字符=%v, 数字=%v, 空格=%v, 其他=%v", charCount.ChCount,
     		charCount.NumCount, charCount.SpaceCount, charCount.otherCount)
     }
     
     ```

   - 



## 4. 命令行参数

1. 使用`os.Args`切片，就可以得到所有的命令行参数
   - 第一个元素是程序名
   - 是一个string的切片

2. **flag包来解析命令行参数**

   - os.Args的方式依赖于参数输入顺序，不利于解析带有指定参数形式的命令行。例如mysql登陆`-u .. -p .. -h.. -port..`

   - 案例：编写代码解析命令行参数，带有指定参数的

     - ```go
       func main() {
       	var user string
       	var pwd string
       	var host string
       	var port int
       
       	// user用来接收用户命令行中输入的 -u 后面的参数值
       	// "u"， 就是 -u 这个指定参数
       	// “”, 第一个参数的默认值
       	// 该字段的说明
       	flag.StringVar(&user, "u", "", "用户名，默认为空")
       	flag.StringVar(&pwd, "pwd", "", "密码，默认为空")
       	flag.StringVar(&host, "ho", "localhost", "主机名，默认为locanhost")
       	flag.IntVar(&port, "port", 3306,"端口号，默认3306")
       
       	// 有一个非常重要的操作，转换，必须调用该方法
       	flag.Parse()
       	
       }
       ```

   - 好处：不依赖输入顺序



## 5. Json

1. **基本介绍**：一种轻量级的数据交换格式，易于人的阅读和编写，也易于机器解析和生成。是主流的数据格式
2. 是用key-value的形式来保存数据
   - 键名写在前面，用双引号`""`包裹，用冒号`:`分隔
   - 多个键值对用逗号分隔

3. www.json.cn，可以验证一个json格式是否正确。
   - 可用来检测复杂json



### 1. 序列化

​	主要介绍结构体，切片，map的序列化，其他数据类型类似。注：对基本数据类型序列化，意义不大

 1. 介绍：序列化是指，将有key-value结构的数据类型，序列化成json字符串的操作

 2. 序列化函数，json包下：`func Marshal(v interface{}) ([]byte, error)`

 3. 案例：

    - ```go
      package main
      
      import (
      	"encoding/json"
      	"fmt"
      )
      
      type Monster struct{
      	Name string
      	Age int
      	Birthday string
      	Salary float64
      	Skill string
      }
      
      func testStruct() {
      	monster := Monster{
      		Name : "niumowang",
      		Age : 500,
      		Birthday: "2011",
      		Salary: 8000.0,
      		Skill: "牛魔拳",
      	}
      
      	//序列化，返回一个[]byte
      	// 为什么要传入指针呢
      	data,err := json.Marshal(&monster)
      	if err != nil{
      		fmt.Println(err)
      	}
      
      	fmt.Println(string(data))
      }
      
      func testMap()  {
      	var a map[string]interface{}
      	// 使用map前要先make
      	a = make(map[string]interface{})
      	a["name"] = "honghaier"
      	a["age"] = 30
      	a["address"] = "hongyadong"
      
      	data,err := json.Marshal(a)
      	if err != nil{
      		fmt.Println(err)
      	}
      
      	fmt.Println(string(data))
      }
      
      func testSlice(){
      	var slice []map[string]interface{}
      
      	var map1 map[string]interface{}
      	map1 = make(map[string]interface{})
      	map1["name"] = "jack"
      	map1["age"] = 3
      	map1["address"] = "北京"
      
      	slice = append(slice, map1)
      
      	var map2 map[string]interface{}
      	map2 = make(map[string]interface{})
      	map2["name"] = "mary"
      	map2["age"] = 91
      	map2["address"] = "西雅图"
      	slice = append(slice, map2)
      
      
      	data,err := json.Marshal(slice)
      	if err != nil{
      		fmt.Println(err)
      	}
      
      	fmt.Println(string(data))
      }
      
      func main() {
      	testStruct()
      	testMap()
      	fmt.Println()
      	testSlice()
      
      }
      
      ```

	4. 









# 10. 单元测试

## 1. 基本介绍

1. Golangg自带一个轻量级的测试框架testing和自带的go test命令，实现单元/性能测试

2. 通过`go test`命令，可以自动执行如下形式的任何函数`func TestXxxx(*testing.T)`
   - 其中Xxxx可以是任何字母数字字符串，但第一个字母不能是[a-z]
   - 被执行的文件名必须是`xxx_test.go`

3. 框架大体执行流程
   - <img src="D:\Desktop\Markdown\Golang\单元测试.png" alt="image-20210506212116747" style="zoom: 67%;" />

## 2. 使用细节

1. 测试文件必须以`_test.go`结尾，且该文件放在与被测试的包相同的包中
2. 测试用例函数必须以Test开头，一般来说就是Test+被测试的函数名，被测试函数名首字母必须大写
3. 测试函数的形参只能是`(t *testing.T)`
4. 一个测试用例文件中，可以有多个测试函数
5. 运行测试用例指令
   - cmd > go test：如果运行正确，无日志；错误时，输出日志
   - cmd > go test -v：正确与否都输出日志
6. 出现错误时，可以用`t.Fatalf`来格式化输出错误信息，并退出程序
7. `t.Logf`方法可以输出相应的日志
8. 测试用例函数，并没有放在main函数中，但也执行了，这就是测试用例的方便之处（见上面原理图——
9. PASS表示成功，FAIL表示失败
10. 测试单个文件，一定要带上被测试的原文件
    - 比如`go test -v cal_test.go cal.go`
11. 测试单个方法
    - `go test -v -test.run 测试函数名`







# 11. goroutine(协程)和channel(管道)

​	**需求引入**：统计1-20000的数字中，哪些是素数

- 传统方法，循环（慢）
- 使用并发或并行的方式，把任务分配给多个goroutine

## 1. goroutine

### 1. 基本介绍

1. 进程和线程
   1. 进程：程序在操作系统中的一次执行过程，是系统进行资源分配和调度的基本单位
   2. 线程：是进程的一个执行实例，是程序执行的最小单元，它是比进程更小的能独立运行的基本单位
   3. 一个进程可以创建和销毁多个线程，同一个进程中的多个线程可以并发执行
   4. 一个程序至少有一个进程，一个进程至少有一个线程

2. 并发和并行
   - 并发：多线程程序在单核上运行（时间片）
   - 并行：多线程程序在多核上运行



### 2. 协程和主线程

1. Go主线程（有人直接称为线程/也可以理解成进程）：一个Go线程上，可以起多个协程。可以理解为，协程是轻量级线程[编译器做了底层优化]
2. **Go协程特点**：
   - 有独立的栈空间
   - 共享程序堆空间
   - 调度由用户控制
   - 协程是轻量级的线程





3. 快速入门

   - 

     ```go
     // 交替输出两种hello
     func say(){
     	for i := 0 ; i < 10; i++ {
     		fmt.Println("hello,world " + strconv.Itoa(i))
     		time.Sleep(time.Second)
     	}
     }
     
     func main() {
     
     	go say() // 开启一个协程
     
     	for i := 0 ; i < 10; i++ {
     		fmt.Println("main() hello,golang " + strconv.Itoa(i))
     		time.Sleep(time.Second)
     	}
     }
     ```

   - 协程执行示意图![image-20210509201453133](D:\Desktop\Markdown\Golang\协程执行示意图.png)



4. **小结**
   1. 主线程是物理线程，直接作用在CPU上的，是重量级的，非常消耗CPU资源
   2. 协程是从主线程开始的，是轻量级的线程，是逻辑态，对资源的消耗很小。
   3. Golang的协程机制是重要的特点，可以轻松开启上万协程。其他编程语言的并发一般是基于线程的，开启线程过多，资源耗费大，因此golang 在并发上具有优势。
   4. 注：**只要主线程执行结束，协程不管是否执行完成，都要结束**
5. **MPG模式：**
   1. M：操作系统主线程（物理线程
   2. P：协程执行需要的上下文（可以理解为资源）
   3. G：协程

<img src="D:\Desktop\Markdown\Golang\gorountine调度模型.png" alt="image-20210509205038671" style="zoom:67%;" />

<img src="D:\Desktop\Markdown\Golang\gorountine调度模型2.png" alt="image-20210509205419862" style="zoom:67%;" />



6. Golang中可以设置运行的CPU数量
   1. 注**：1.8后，默认程序运行在多核上，可以不设置了**
   2. `runtime.NumCPU()`：返回机器的逻辑CPU个数
   3. `runtime.GPMAXPROCS`：设置可同时执行的最大CPU

### 3. 协程并发（并行）资源竞争问题

1. 案例：如果开多个协程，像一个map写入数据，就可能发生并发写入的问题
2. 运行某个程序时，可以在编译时，增加参数`-race`，就可以知道是否存在资源竞争问题

### 4. 全局互斥锁

1. `var lock sync.Mutex`：声明一个互斥锁
   - `lock.Lock()`加锁
   - `lock.Unlock()`解锁
2. 低水平使用互斥锁，高水平用channel 



## 2. channel

### 1. 基本介绍

1. 为什么需要channel
   1. 之前使用全局变量加锁同步，解决goroutine通讯，不完美
   2. 主线程等待所有goruntine全部完成的时间很难确定，（因为主线程如果不等待，一旦其结束，协程也强制结束）
   3. 若主线程休眠长，会检测等待时间；若休眠短，则可能其结束时，goruntine还在工作，但随着主线程的退出而销毁
   4. 不利于多个协程对全局变量的读写操作

2. channel**介绍**
   1. channel本质就是一个数据结构——队列
   2. 数据是先进先出[FIFO：first in first out]
   3. 线程安全，多goroutine访问时，不需要加锁，就是说channel本身就是线程安全的
   4. channel是有类型的，一个string的channel只能存放string类型的数据

### 2. 基本使用

1. **定义**：`var 变量名 chan 数据类型`

   - `var intChan chan int`：用于存放int数据
   - `var mapChan chan map[int]string`
   - `var perChan chan Person`
   - `var intChan chan *Person`

2. 说明：

   - channel是**引用类型**
   - channel**必须初始化才能写入数据**，即make后才能使用
   - 有类型

3. 入门案例：

   - ```go
     // 1. 创建一个可以存放3个int的管道	
     var intChan chan int= make(chan int, 3)
     
     // 2. 输出管道，得到一个地址，指向管道队列的头
     fmt.Println(intChan)
     
     // 3.向管道写入数据
     intChan<- 10
     num := 211
     intChan<- num
     intChan<- 50
     
     // 4. 看看管道的长度和cap
     // cap就是我们make时指定的容量
     // 并且channel是不会自动扩容的
     // 因此写入数据时，不能超过其容量
     fmt.Println(len(intChan), cap(intChan))
     
     // 5. 取一个数据
     var num2 int
     num2 = <-intChan // 10
     num3 := <-intChan // 211
     
     // 6. 没有使用协程的情况下，如果管道数据已经全部取出，再取就会报告deadlock
     ```

4. 注意事项

   - 只能存放指定的数据类型
   - 数据放慢后，不能再放入
   - 如果从Channel取出数据后，可以继续放入
   - 没有使用协程情况下，如果channel数据取完了，再取就会报deadlock
   - `<-intchan`可以不用变量接收管道的数据
   - 如果希望管道里面的数据类型有多种，声明管道的时候，类型用`interface{}`

5. 小案例：

   - ```go
     allChan := make(chan interface{}, 3)
     
     allChan<- 10
     allChan <- "jom"
     cat := Cat{"xii",4}
     allChan <- cat
     
     <-allChan
     <-allChan
     
     newCat := <-allChan
     // 这里输出，运行时可以判断出newCat是Cat类型的
     
     fmt.Printf("%T",newCat)
     
     // 但由于channel中的数据类型为interface{}
     // 编译器认为newCat是interface的变量，没有方法和字段
     // 因此编译会报错
     fmt.Println(newCat.Name)
     // 使用类型断言就可以取出name
     a := newCat.(Cat)
     fmt.Println(a.Name) // 正确
     
     ```



### 3. channel的关闭和遍历

1. channel的关闭

   - 使用内置函数close可以关闭channel
   - 关闭后，channel只能读，不能写
   - `close(channel)`

2. channel的遍历 

   - 支持for-range的方式遍历

   - 注意：

     - 遍历时，如果channel没关闭，则会出现deadlock错误

       - 没关闭，也可以成功读出全部数据，但是最后会deadlock

     - 如果channel已经关闭，则会正常遍历，遍历完后，就会退出遍历

     - ```go
       close(intChan)
       for v := range intChan {
           
       }
       ```

     - 管道没有下标的概念，因此我们想取第三个值，一定要先把前两个值取出

   - 不能用普通for循环遍历，因为取出数据时，len(chan)在变化（除非先用变量获得当前channel的len，然后再遍历）

     - ```go
       length := len(intChan)
       for i := 0; i < length; i++{
           
       }
       ```

### 4. 协程配合管道综合案例

1. 例子1：
   1. 要求：

      - 开启一个writeData协程，向管道中写入50个int
      - 开启一个readData协程，从管道中读取writeData写入的数据
      - 注：两个协程操作同一个管道
      - 主线程需要等到上面两个协程都工作完成，才能退出【利用一个额外的管道实现通信】

   2. 思路分析：

      - 需要利用一个额外的管道，让主线程直到，读协程已经工作完毕

   3. 代码

   - ```go
     package main
     
     import "fmt"
     
     
     func writeData(intChan chan int) {
     	for i := 1; i <= 50; i++ {
     		// 放入数据
     		intChan <- i
     		fmt.Println("writeData ", i)
     	}
     	// 关闭
     	close(intChan)
     }
     
     func readData(intChan chan int, exitChan chan bool) {
     	for {
     		// 如果写慢读快，读会阻塞在这里，直到有数据传入
     		// 这种方式就比使用全局变量标记，不断循环查看的方式要好
     		v, ok := <-intChan
     		if !ok {
     			break
     		}
     		fmt.Println("readData ", v)
     	}
     	exitChan <- true
     	close(exitChan)
     }
     
     func main() {
     
     	intChan := make(chan int, 50)
     	exitChan := make(chan bool, 1)
     
     	go writeData(intChan)
     	go readData(intChan, exitChan)
     
     	// 阻塞到readdata写入exitChan
     	// 主程序直到读完数据了，才能退出
     	for {
     		 _, ok := <-exitChan
     		 if !ok{
     		 	break
     		 }
     	}
     }
     
     ```

2. 例子2——阻塞：

   1. 把上面例子中，intChan的cap改成50
   2. 然后注释掉`go readData(intChan, exitChan)`
   3. 代码会阻塞在writeData的ch <- i
   4. _, ok := <-exitChan也会死锁，因为编译器发现exitchan永远关不上，也读不到数据
   5. 注：即便管道写满了，先阻塞，如果还有其他协程在读，就不会报deadlock；但是如果没有其他协程读了，就会deadlock
      - 总结：读写操作是异步的，速度不同不会报错

3. 协程求素数（求1-200000中，哪些是素数）

   - 要求和思路：

     - <img src="D:\Desktop\Markdown\Golang\协程求素数思路.png" alt="image-20210511003946217" style="zoom:67%;" />

   - 代码：

     - ```go
       package main
       
       import "fmt"
       
       func putNum(intChan chan int) {
       	for i := 0; i < 8000; i++ {
       		intChan <- i
       	}
       
       	close(intChan)
       }
       
       func primeNum(intChan chan int, primeChan chan int, exitChan chan bool) {
       	var num int
       	var flag bool
       	var ok bool
       	for {
       		num, ok = <-intChan
       		if !ok { // intChan取不到了
       			break
       		}
       		// 判断num是不是素数
       		flag = true
       		for i := 2; i < num; i++ {
       			if num%i == 0 {
       				flag = false
       				break
       			}
       		}
       		if flag {
       			primeChan <- num
       		}
       	}
       
       	fmt.Println("有一个协程，因为取不到数据退出了")
       	// 这里还不能关闭primeChan
       	// 向exitChan写入true
       	exitChan <- true
       }
       
       func main() {
       
       	intChan := make(chan int, 1000)
       	primeChan := make(chan int, 1000)
       	exitChan := make(chan bool, 4)
       
       	// 开启一个协程，向intchan放入1-8000个数
       	go putNum(intChan)
       
       	// 开启4个协程，从intChan取出数据，并判断是否为素数，
       	// 如果是，就放到primeChan
       	for i:= 0; i < 4; i++ {
       		go primeNum(intChan, primeChan, exitChan)
       	}
       
       
       	go func() {
       		for i := 0; i < 4; i++{
       			<-exitChan
       		}
       
       		// 当从exitchan取出了4个结果，就可以关闭primeChan了
       		close(primeChan)
       	}()
       
       	for {
       		res, ok := <-primeChan
       		if !ok{
       			break
       		}
       		fmt.Println("素数 = ", res)
       	}
       
       }
       ```

     - 

### 5. 使用细节

1. **管道可以声明为只读或只写**

   1. 默认情况下，管道是双向的
   2. 声明为只写：`var intChan chan<- int`
   3. 声明为只写：`var intChan <-chan int `
   4. 案例：<img src="D:\Desktop\Markdown\Golang\管道只读或只写案例.png" alt="image-20210512002504493" style="zoom:67%;" />
   5. 也就是说，可以声明为默认管道，在函数定义参数时要求是只读或只写的，把默认的双向管道传入相关函数，在函数内部完成只读或只写的功能

2. 使用select，可以解决从管道取数据的阻塞问题

   - ```go
     	// 使用select可以解决从管道取数据的阻塞问题
       
       	intChan := make(chan int, 10)
       	for i := 0; i < 10; i++ {
       		intChan <- i
       	}
       
       	stringChan := make(chan string, 5)
       	for i := 0; i < 5; i++ {
       		stringChan <- "hello" + fmt.Sprintf("%d", i)
       	}
       
       	// 传统方法，遍历管道时，如果管道不关闭，会导致deadlock
       	// 实际开发中，有可能我们不好确定什么时候应该关闭管道
       	// 可以使用select方式
       	for {
       		select {
       		// 注意：这里，如果intchan一直没有关闭
       		// 也不会一直阻塞而deadlock
       		// 会自动到下一个case匹配
       		case v := <-intChan:
       			fmt.Printf("从intChan读取的数据%d\n", v)
       		case v := <-stringChan:
       			fmt.Printf("从stringChan读取的数据%s\n", v)
       		default:
       			fmt.Println("tmd, 都取不到，可以加自己的逻辑")
       			return
       
       		}
       	}
     ```

   - **疑问：**为什么输出的顺序，是int和string随机读出来的，不应该按顺序吗

3. goroutine中，可以使用recover，解决协程中panic，使得一个协程出错，不影响其他协程（和主线程）

   ```go
   	// 使用select可以解决从管道取数据的阻塞问题
   
   	intChan := make(chan int, 10)
   	for i := 0; i < 10; i++ {
   		intChan <- i
   	}
   
   	stringChan := make(chan string, 5)
   	for i := 0; i < 5; i++ {
   		stringChan <- "hello" + fmt.Sprintf("%d", i)
   	}
   
   	// 传统方法，遍历管道时，如果管道不关闭，会导致deadlock
   	// 实际开发中，有可能我们不好确定什么时候应该关闭管道
   	// 可以使用select方式
   	for {
   		select {
   		// 注意：这里，如果intchan一直没有关闭
   		// 也不会一直阻塞而deadlock
   		// 会自动到下一个case匹配
   		case v := <-intChan:
   			fmt.Printf("从intChan读取的数据%d\n", v)
   		case v := <-stringChan:
   			fmt.Printf("从stringChan读取的数据%s\n", v)
   		default:
   			fmt.Println("tmd, 都取不到，可以加自己的逻辑")
   			return
   
   		}
   	}
   ```

   

# 12. 反射



# 13. 网络编程

## 1. 基础

1. **网络编程有两种**
   
   - TCP socket编程：底层是基于Tcp/ip协议，是网络编程的主流，比如QQ
   - b/s结构的http编程：浏览器访问服务器时，就是http，底层依然是tcp socket实现的。
   
2. **基础知识**

   1. 只要是做服务程序，都必须监听一个端口
   2. 该端口就是其他程序和该服务通信的通道
   3. 一台电脑有65535个端口1-65535

   4. 一旦一个端口被某个程序监听（占用），其他程序就不能在该端口监听

3. **端口分类**：

   - 0号是保留端口
   - 1-1024是固定端口，又叫有名端口，也就是被某些程序固定使用
     - 22：ssh远程登录协议
     - 23：Telnet使用
     - 21：ftp
     - 25：smtp
     - 80：iis
     - 7：echo
   - 1025-65535是动态端口，可以给我们使用

4. 端口使用注意

   - 计算机中，尤其是做服务器，尽量少开端口
   - 一个端口只能被一个程序监听
   - `netstat -an`：可以查看本机有哪些端口在监听
   - `netstat -anb`：查看监听端口的pid，结合任务管理，关闭不安全的端口

## 2. TCP socket编程

### 1. 快速入门

1. 服务端处理流程
   - 监听端口，比如8888
   - 接受客户端的tcp链接，简历客户端和服务器端的链接
   - 创建goroutine，处理该链接的请求（通常客户端会通过链接发送请求包）

2. 客户端处理流程
   - 建立与服务端的链接
   - 发送请求数据[终端]，接收服务器端返回的结果数据
   - 关闭链接



3. 入门案例
   1. 程序示意图<img src="D:\Desktop\Markdown\Golang\SOCKET服务示意图.png" alt="image-20210515204305148" style="zoom: 67%;" />

   2. 服务端功能：
      1. 编写一个程序，在8888端口监听
      2. 可以和多个客户端创建链接
      3. 链接成功后，客户端可以发送数据，服务器端接受数据，并显示在终端上
      4. 先用telnet来测试，然后编写客户端程序来测试
      
   3. 客户端功能：

      1. 写程序，链接到服务器端的8888端口
      2. 可以发送单行数据，然后退出
      3. 能通过终端输入数据（输入一行发送一行），并发送给服务端
      4. 终端输入exit，退出程序

   4. 代码展示

      - server.go

      - ```go
        package main
        
        import (
        	"fmt"
        	"net"
        )
        
        func process(c net.Conn) {
        	// 循环接收客户端发送的数据
        	defer c.Close() // 关闭conn
        
        	for {
        		// 创建一个新的切片
        		buf := make([]byte, 1024)
        
        		// 1. 等待客户端通过conn发送信息
        		// 2. 如果客户端没有通过write发送，协程就阻塞在这里
        		//fmt.Printf("服务器在等待客户端%s的信息\n" ,c.RemoteAddr().String())
        		n, err := c.Read(buf) // 从conn读取
        		if err != nil {
        			fmt.Println("客户端已退出")
        			return // !!!
        		}
        		// 3. 显示客户端发送的内容到服务器的终端
        		// buf里面的n一定要写，显示实际收到的内容
        		// 不然读到1024，后面可能是乱七八糟的数据
        		fmt.Print(string(buf[:n])) //!!!
        	}
        }
        func main() {
        	fmt.Println("服务器开始监听")
        
        	// 返回在一个本地网络地址laddr上监听的Listener。
        	// listener是一个接口
        	// 网络类型参数net必须是面向流的网络
        	// 1. tcp表示使用的网络协议
        	// 2. 表示在本地监听8888端口
        	listen, err := net.Listen("tcp", "0.0.0.0:8888")
        	if err != nil {
        		fmt.Println("listen err=", err)
        		return
        	}
        
        	// 关闭该接口，并使任何阻塞的accept操作，都会不再阻塞，并返回值
        	// 延时关闭
        	defer listen.Close()
        	//fmt.Printf("%v", listen)
        
        	// 循环等待客户端链接我们
        	for {
        		// Accept等待并返回下一个连接到该接口的连接
        		fmt.Println("等待客户端链接")
        
        		// 返回一个网络连接的接口对象
        		conn, err := listen.Accept()
        		if err != nil {
        			fmt.Println("accept err=", err)
        		} else {
        			fmt.Printf("accept success con = %v , 客户端ip=%v",
        				conn, conn.RemoteAddr().String())
        		}
        
        		go process(conn)
        	}
        }
        
        ```

      - client.go

      - ```go
        package main
        
        import (
        	"bufio"
        	"fmt"
        	"net"
        	"os"
        	"strings"
        )
        
        func main(){
        	conn, err :=net.Dial("tcp", "127.0.0.1:8888")
        	if err != nil {
        		fmt.Printf("连接失败, %v", conn)
        		return
        	}
        
        	// 1. 客户端发送数据
        	reader := bufio.NewReader(os.Stdin) // os.stdin代表标准输入[终端]
        
        	// 循环从终端读取一行用户输入，并发送给服务器
        	// 遇到exit退出
        	for {
        		line, err := reader.ReadString('\n')
        		if err != nil {
        			fmt.Println("read err")
        		}
        
        		line = strings.Trim(line, "\r\n")
        		if line == "exit" {
        			fmt.Println("客户端退出")
        			break
        		}
        
        		// 再将line发送给服务器
        		_, err = conn.Write([]byte(line + "\n"))
        		if err != nil {
        			fmt.Println("conn 写失败")
        		}
        	}
        }
        ```

        



# 14. Go与Redis

## 1. Redis数据类型和CRUD

### 1. String

1. 介绍：redis最基本的数据类型，一个key对应一个value

   - 例如`str1 := "hello"`，str1就是key，后面的值就是value
   - string类型是二进制安全的，除了普通的字符串，也可以存放图片等数据
   - redis中字符串value最大是512M

2. 字符串CRUD

   1. `set key value`：key存在就是修改，不存在就是添加

   2. `get key`

   3. `del key`

   4. `setex(set with expire) key seconds value`：将key的生存时间设为seconds

      - 如果key已经存在，setex命令将覆盖旧值

      - 该命令类似于下面两个命令

      - ```sql
        set key value
        expire key seconds
        ```

   5. `mset key value [key value ...]`：同时设置一个或多个key-value对

      - `mget`：同时获取多个key-value



### 2. Hash

1. 介绍：是一个键值对集合，相当于map[string]string
   - redis的hash是一个string类型的field和value的映射表，特别适合用于存储对象

2. CRUD
   1. `hset key field value`
   2. `hget key field`
   3. `hgetall key`
   4. `hdel key field`
   5. `hmset`和`hmget`：一次性设置多个field的值和返回多个field的值
   6. `hlen`：统计一个hash有多少个元素
   7. `hexists key field`：哈希表map中，给定的域field是否存在



### 3. List

1. 介绍：简单的字符串列表，按照插入顺序，可以添加元素到头部（左边）或尾部（右边）
   - 本质是个链表
   - 元素有序
   - 值可以重复
2. CRUD
   1. `lpush key value1 value2`：数据从左边插入
   2. `rpush`：数据从右边插入
   3. `lrange key start end` ：从左到右取出指定区间的元素，end若为-1，代表取到最后一个元素，
   4. `lpop key`：从左边弹出元素
   5. `rpop key`
   6. `del key`：删除list
   7. `lindex key index`：返回下标为index 的元素（从左到右，从0开始）
   8. `llen key`：  
   9. 注：如果全部的值都pop出了，list也不存在了



### 4. set

1. 介绍：string类型的无序集合，底层是hashtable数据结构，
   - 无序的
   - 值不能重复
2. CRUD
   1. `sadd key value1 value2`：可添加多个
   2. `smembers`：取出所有值
   3. `sismenber`：判断值是否是成员
   4. `srem`：删除指定值

## 2. Go连接到redis

### 1. 入门案例：

 注：主要就是通过conn.Do来传入redis命令

还有go-redis/redis库，也可以使用

- 操作string

- ```go
  package main
  
  import (
  	"fmt"
  	"redigo/redis"
  )
  
  func main() {
  	c, err := redis.Dial("tcp", "127.0.0.1:6379")
  
  	if err != nil {
  		fmt.Println("connect failed ",err)
  		return
  	}
  	
  	defer c.Close()
  	// c.do方法返回的是interface类型的
  	// 可以用redis提供的类型转换方法来把结果转为string或其他类型
  	// 直接强转或用类型断言会失败
  	r, err := redis.String(c.Do("get", "name"))
  	if err != nil {
  		fmt.Println("set failed ",err)
  		return
  	}
  	fmt.Println(r)
  }
  
  ```

- 操作hash

- ```go
  package main
  
  import (
  	"fmt"
  	"redigo/redis"
  )
  
  func main() {
  	c, err := redis.Dial("tcp", "127.0.0.1:6379")
  
  	if err != nil {
  		fmt.Println("connect failed ",err)
  		return
  	}
  
  	defer c.Close()
  	// c.do方法返回的是interface类型的
  	// 可以用redis提供的类型转换方法来把结果转为string或其他类型
  	// 直接强转或用类型断言会失败
  	r, err := redis.Strings(c.Do("hgetall","map"))
  	if err != nil {
  		fmt.Println("set failed ",err)
  		return
  	}
  	
      // r是一个[]string
  	for _,v := range r {
  		fmt.Println(v)
  	}
  }
  
  
  ```

- 

### 2. 连接池

1. 介绍
   - 事先初始化一定数量的链接，放入到链接池
   - 当Go需要操作redis时，直接从redis链接池取出链接即可
   - 这样可以节省临时获取redis链接的时间，从而提高效率
   - 注：思想类似于线程池

2. 使用案例

   - 核心代码

     - ```gO
       package main
       
       import (
       	"redigo/redis"
       )
       
       func main() {
       	var pool *redis.Pool
       
       	pool = &redis.Pool{
       		MaxIdle: 8, // 最大空闲链接数
       		MaxActive: 0, // 表示和数据库的最大链接数，0表示没有限制，但实际上操作系统有限制
       		IdleTimeout: 100, // 最大空闲时间
       		Dial: func() (redis.Conn, error) { // 初始化链接
       			return redis.Dial("tcp","localhost:6379")
       		},
       	}
       	
       	c := pool.Get() // 从池子中取出一个链接
           defer c.close()
           pool.Close()
       }
       
       
       ```

     - 























































































































# 100. 踩坑记录

## 1. Json

1. Unmarshal时，**对于引用类型，需要传入其指针**

   ```golang
   callbackargs := “{
     \"type\": \"pgc_music\",
     \"pgc_retrace\": \"1\",
     \"music_id\": \"6984272955597392684\",
     \"trigger_time\": \"1626160922480\",
     \"use_await_event\": \"1\"
   }”
   callbackArgs := &model.WordflowCallbackMsg{}
   // 如果写成callbackArgs := model.WordflowCallbackMsg{}
   // 则会报错：json: Unmarshal(non-pointer model.WordflowCallbackMsg)
   
   err := ajson.Unmarshal([]byte(callbackargs), callbackArgs)
   if err != nil {
     fmt.Printf("???, err = %v", err)
   }
   ```

   - 上面的例子中，如果写成`callbackArgs := model.WordflowCallbackMsg{}`，则会报错：`json: Unmarshal(non-pointer model.WordflowCallbackMsg)`









# 补充知识

1. `fmt.Sprinf`：用在表达式中，可以直接规定浮点数运算的结果保留小数点几位

   - ```go
     res := fmt.Sprintf("%.2f", 5 / 3)
     // 这样得到的res是四舍五入保留两位小数的值，提前格式化
     ```

     

# 项目开发流程

![image-20210505165832474](D:\Desktop\Markdown\Golang\项目开发流程)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             

