# Java学习笔记-基本程序设计结构


[Java 文档下载](http://www.oracle.com/technetwork/java/javase/downloads)

## 使用集成开发环境
### Eclipse
#### 新建项目
1. File-New-Project
2. 对话框中选择`Java Project` 

![配置eclipse工程](配置eclipse工程.png)

3. 点击`Finish`


### Virtual Studio Code
1. 安装扩展包`Java Extension Pack`
2. `Ctrl-Shift-D` 根据提示生成`launch.json`配置文件
3. `F5`运行程序

## 第一段Java程序
```Java
// Welcome/Welcome.java
public class Welcome{
    public static void main(String [] args){
        String greeting = "Welcome to Spaceack's blog";
        System.out.println(greeting);
        for(int i=0; i<greeting.length(); i++)
            System.out.print("=");
        System.out.println();
    } 
}
```
#### 程序解读
1. 关键字`public`称为`访问修饰符（access modifier）`用于控制程序其它部分对这段代码的访问级别。
2. 关键字`class`表明Java程序中的全部内容都包含在类中。
3. `class`后面是类名，命名规范类名首字母大写，建议使用骆驼命名法。
4. 源代码的文件名需与**公共类名**相同，并用`.java`作为扩展名。
5. `main`方法必须声明为`public`
6. Java中所有的函数都属于某个类的方法，因此`main`方法必须有一个外壳类。每个Java应用程序都必须有一个`main`方法, 声明格式：
    ```java
    public class ClassName{
        public static void main(String[] args){
            program statements
        }
    }
    ```

7. `void`表示这个方法没有返回值。如果`main`方法正常退出，Java应用程序退出代码为0。若想终止程序时返回其他代码，需要调用`System.exit`方法。
8. 每个程序块使用`{}`括号包裹，每条语句使用`;`分号结束。
9. 输出语句使用了`System.out`对象，并调用了`println`方法。
10. `.`点号用于调用方法，通用语法为`object.method(parameters)`


![编译运行Welcome](编译运行Welcome.png)
### 易错点
1. 需要注意大小写， 类名首字母大写 `Welcome`
2. 编译时只需指定类名`javac Welcome`,不要带扩展名`.java`或`.javac`

## 注释
1. 单行注释`//`
2. 多行注释`/*   */`
3. 文档注释`/**  */` 可以生成文档
    ```java
    /**
     * This is the first sample program
     * @version 1.0 2016-09-03
     * @author Spaceack
     * /
    ```

## 数据类型
Java中规定8种基本类型（primitive type）4种整型，2种浮点型，1种表示Unicode编码的字符型，和一种表示真值的boolean类型。
### Java整型
|类型|存储|取值|科学|
|-|-|-|-|
|byte|1字节|-128~127|2^7|
|short|2字节|-32768~32767|2^15|
|int|4字节|-2147483648～2147483647（错过20亿）|2^31|
|long|8字节|-9223372036854775808~9223372036854775807|2^63|

1. 长整型数值有一个后缀`L`或`l`(4000000000L)
2. 十六进制数值有一个前缀`0x`或`0X`(0xCAFE)
3. 八进制有个前缀`0`（010 表示对应八进制中的8）
4. 二进制数值前缀`0b`或`0B`（0b1001就是9）
5. 数字字面量加入下划线使之更易读：1_000_000（0b1111_0100_0010_0100_0000）表示一百万。

### 浮点类型
|类型|存储|取值|
|-|-|-|
|float|4字节|约 ±3.40282347E+38F(有效位数6～7位)|
|double|8字节|约 ±1.79769313486231570E+308(有效位数15位)|

1. `float`类型的数值有一个后缀`f`或`F`。没有后缀的浮点数默认为`double`类型
2. 表示溢出和出错情况的三个特殊的浮点数值
 - 正无穷大，对应常量`Double.POSITIVE_INFINITY`
 - 负无穷大,`Double.NEGATIVE_INFINITY`
 - NaN, `Double.NaN`

    eg: 一个正整数除0的结果为正无穷大。计算0/0或者负数平方根的结果为NaN。

3. 使用`Double.isNaN()`方法判断是否等于`Double.NaN`。
4. 浮点数值表示使用二进制系统表示，会有误差。金融计算应使用`BigDecimal`类。

### char类型
1. 使用单引号`''`包裹,例如`'A'`是编码值为65所对应的字符常量。与被双引号包裹的`"A"`不同，它是包含一个字符`A`的字符串。
2. char类型可以表示十六进制值，范围从`\u0000`到`\Uffff`。例如`\u2122`表示注册符号`™`,`\u03C0`表示希腊字母`π`。
3. 转义序列`\u`可以出现在加引号的字符常量或字符串之外（其他转义序列不可以）例如 `\u005B\u005D`是`[]`的编码。
    ```java 
    public static void main(String \u005B\u005D args)
    ```
4. 其它特殊字符转义序列

    |转义序列|名称|Unicode值|
    |-|-|-|
    |\b|退格|\u0008|
    |\t|制表|\u0009|
    |\n|换行|\u000a|
    |\r|回车|\u000d|
    |\\"|双引号|\u0022|
    |\\'|单引号|\u0027|
    |\/\/|反斜杠|\u005c|

5. `System.out.println("\u0022+\u0022");`的输出会是`""`吗？不是的， 会输出一个空字符串` `。
6. 需要小心注释中的`\u`, `// Look inside c:\users` 会产生语法错误，因为\u后面并未跟着4个十六进制数。

#### Unicode
码点（code point）: 指与一个编码表中的某个字符对应的代码值。

在Unicode标准中，码点采用16进制书写，并加上前缀`U+`,eg`U+0041`是`A`的码点。

Unicode码点可以分成17个代码级别（code plane）。第一个代码级别称为**基本的多语言级别(basic multilingual plane)**， 码点从`U+0000`到`U+FFFF`,包括经典的Unicode代码。其余的16个级别码点从`U+10000`到`U+10FFFF`，包括一些**辅助字符（supplementary character）**

`UTF-16`编码采用不同长度的编码表示所有的`Unicode`码点。在**基本的多语言级别**中每个字符用16位表示，通常被称为**代码单元（code unit）**。**辅助**字符采用一对连续的**代码单元**进行编码。这样构成的编码值落入**基本的多语言级别**中空闲的2048字节内，通常被称为**替代区域（surrogate area）**。

在Java中，char类型描述了UTF-16编码中的一个代码单元。

强烈建议不要在程序中使用char类型。建议使用字符串作为抽象数据类型处理。


### boolean类型
1. boolean类型有两个值：`false`和`true`,用来判定逻辑条件。
2. 整型和布尔值之间不能相互转换。

## 变量
1. Java中每个变量都有1个类型（type）,在声明变量时，变量的类型位于变量之前。以分号结束。
2. 可以使用任何有意义的Unicode字符组成变量名。若想知道该Unicode字符是否可以用作变量名，可以使用`Character`类的`isJavaIdentifierStart`和`isJavaIdentifierPart`方法来检查。
3. 不要使用`$`字符，它只用在Java编译器或其它工具生成的名字中。
4. 建议逐一声明（各一行）可以提高程序的可读性。
5. 变量名建议小写字母，多个单词组成的变量名从第二个单词开始首字母大写。

### 变量初始化
1. 声明变量后，必须使用赋值语句对变量显式初始化。
2. 变量的声明尽可能地靠近变量第一次使用的地方。
### 常量
1. 利用关键字`final`指示常量。eg:
    ```java
    final int MAX_SIZE = 25;
    ```
2. 关键字`static final`可以设置一个类常量，允许一个类中的多个方法使用,常量还被声明为public， 其它的类也可以使用这个类常量：
    ```java
    public static final int MAX_SIZE = 25;
    ```
3. 被赋值后不能再更改。
4. 建议常量名使用全大写。

## 运算符

### 算术运算符
|符号|含义|使用|例子|
|-|-|-|-|
|+|加|
|-|减|
|*|乘|
|/|除|操作数都是整数时，表示整数除法，否则表示浮点数除法|50/2=7, 15.0/2=7.5|
|%|求余(取模)||15%2=1|

### 数学函数
```java
double x = 4;
// sqrt方法 求平方根 
double y = Math.sqrt(x); // 2.0
// pow方法 指数运算 y的x次方
double p = Math.pow(y, x) //16.0
// foorMod方法 取模运算
double m = Math.floorMod(4, 3) //1.0
```

```java
// 使用静态导入，调用时省略Math
import static java.lang.Math;
System.out.println(PI);
double s = sin(toRadians(30));
System.out.println(PI);
```

### 类型转换
![类型间合法转换](yuque_diagram_type.png)
 - 虚箭头转换可能会有精度损失
 - 两个操作数，需要转换为同一类型才能运算。

### 强制类型转换
- 使用圆括号`()`包裹要转换的目标类型，后面紧跟需要变换的变量名。
```java
double x = 6.66; 
int nv = (int) x; // 6 截断小数部分变为整型。
int rv = (int) Math.round(x); // 7 使用round 进行舍入运算，round返回类型为long
```
```java
int x = 0;
x += 3.5;  // 3 
// 会执行强制转换，实际为(int)（x + 3.5）
```

### 自增运算符
- “后缀”形式
 ```java
 int n = 2;
 int b = 2 * m++; // n is 3, b is 4; 
 ```

- “前缀”形式, 先加1。
```java
int m = 2;
int a = 2 * ++m; // m is 3, a is 6
```

- 建议不在表达式中使用`++`，会让人迷惑。
 
### 关系和布尔运算符
|符号|含义|举例|
|-|-|-|
|`==`|相等|
|`!=`|不相等|
|`<`|小于|
|`>`|大于|
|`<=`|小于等于|
|`>=`|大于等于|
|`&&`|逻辑与|
|`||`|逻辑或|
|`? :`|三元操作符| condition ? expression1 : expression2|

 - `&&`与`||` 按照`短路`方式来求值


### 位运算符
|符号|含义|举例|
|-|-|-|
|`&`|and|
|`|`| or |
|`^`|xor|
|`~`|not|
|`>>`|左移|
|`<<`|右移|

 - `&`与`|` **不**按照`短路`方式来求值

- 使用掩码技术可以得到整数中的各个位。
```java
	int n = 0b10000;
	int fourthBitFromRight = (n & 0b1000) / 0b1000;  // 0

    int n = 0b11000;
	int fourthBitFromRight = (n & 0b1000) / 0b1000;  // 1

    int n = 0b10000;
	int fourthBitFromRight = (n & (1<<3)) >>3;  // 0

    int n = 0b11000;
	int fourthBitFromRight = (n & (1<<3)) >>3;  // 1
```
### 括号与运算符级别
![括号与运算符级别](表6-7.jpg)

### 枚举类型
```java
enum Size {SMALL, MEDIUM, LARGE, EXTRA_LARGE};
Size s = Size.MEIUM;
```
## 字符串
- Java字符串就是Unicode字符序列。
- JAVA没有内置的字符串类型，而是使用标准库中的一个预定义类。
- 每个由双引号括起来的字符串都是String类的一个序列。

```java
String name = "Spaceack";
```
### 子串
- 使用`substring`方法提取子串
```java
String name = "Spaceack";
String subs = name.substring(0,5);  //"Space"

- 子串长度 即 5-0 = 5

```
### 拼接
- 使用`+`号 拼接两个字符串
- 多个字符串使用某个定界符连接，可以使用`join`静态方法
```java
String all = String.join("-", "2016", "10", "01"); //2016-10-01
```
### 不可变字符串
### 检测字符串相等
不要使用`==` 检测字符串相等。

使用`equals`方法`s.equals(t)`, 相等返回`true`, 否则返回`false`

`equalsIgnoreCase`方法，不区分大小写。

### 空串与Null串
空串是长度为0的字符串。
使用 `if (str.length() == 0)` 或 `str.equals("")`检测。

使用`if (str == null)`检测Null串

使用`if (str != null && str.length() != 0 )` 检测字符串既不是null, 也不为空串。
### 码点与代码单元
- `length`方法返回采用UTF-16编码表示的给定字符串所需要的代码单元数量
    ```java
    String name = "Spaceack👽";
    int n = name.length();// 10
    ``` 
 - 获取实际长度，即码点数量，可调用：
    ```java
    int cpCount = name.codePointCount(0, name.length()); // 9
    ```
 - `s.charAt(n)` 返回位置n的代码单元，n介于0~`s.length() - 1` 之间
    ```java
    char first = name.charAt(0); // S
    char last = name.charAt(9); // ?
    ```
- 得到第i个码点，可调用：
    ```java
    int i = 8;
    int index = name.offsetByCodePoints(0, i);
    int cp = name.codePointAt(index); // 8128125
    ```
## 输入输出
## 控制流程
## 大数值
## 数组

## 术语
```
码点（code point）: 指与一个编码表中的某个字符对应的代码值。

面向对象程序设计（Object-Oriented Programming, OOP）
lambda表达式（lambda expression）
异常处理（excepion handling）
抽象窗口工具包（abstract window toolkit, AWT）

```
