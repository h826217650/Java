
# java 虚拟机

 首先，了解下java执行过程  如图
![](/Users/huxiaoyan/Desktop/java的执行过程.png)


.class文件不是直接被系统加载后直接在cpu上执行的，而是被一个叫做虚拟机的进程托管的。  

* java文件通过编译生成.class file  
* 通过类加载器，加载必要的class文件，包括jdk的基础类  
* 然后通过执行引擎执行class文件中的字节码指令  
* 在JVM执行过程中要分配内存创建对象

![java 执行引擎结构图](/Users/huxiaoyan/Desktop/java执行引擎.jpg)


# java内存机制
https://www.jianshu.com/p/f29f52726c87

## java内存划分
java内存分配结构图： 其中PC register，stack和本地方法栈是线程独有，堆，方法区和常量池是JVM实例中所有线程共享。
![](/Users/huxiaoyan/Desktop/java内存分配.png)
### 堆  
  * 存放：new创建的对象和数组（基本类型包装类存储在堆的常量池中）  
  * 内存释放：有JVM自动垃圾回收器完成。当没有引用指向该对象时，对象才可以通过GC进行内存释放。
  * Java堆可以细分为：新生代、老生代；  

### 栈   
  * 存放：局部变量（基本类型变量和对象的引用），运行环境  
  * 内存释放：变量超过作用域时 自动释放.  
  * 栈数据可共享
  
  编译器先处理 int a = 3；首先它会在栈中创建一个变量为 a 的引用，然后查找有没有字面值为3的地址，没找到，就开辟一个存放3这个字面值的地址，然后将 a 指向3的地址。接着处理 int b = 3；在创建完 b 这个引用变量后，由于在栈中已经有3这个字面值，便将 b 直接指向3的地址。这样，就出现了 a 与 b 同时均指向3的情况。 定义完 a 与 b 的值后，再令 a = 4；那么，b 不会等于4，还是等于3。在编译器内部，遇到时，它就会重新搜索栈中是否有4的字面值，如果没有，重新开辟地址存放4的值；如果已经有了，则直接将 a 指向这个地址。因此 a 值的改变不会影响到 b 的值。
 
### 程序计数器  
 * 存放：当前线程的执行的字节码的行号指示器
### 本地栈  
 *  存放：为本地native方法服务
### 方法区  
 * 存放：被虚拟机加载的类信息，常量，静态变量和即时编译编译后的代码

### 常量池
 * 存放：用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放  
Java语言并不要求常量一定只有编译期才能产生，也就是并非预置入CLass文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用比较多的就是String类的intern()方法。

## java基本类型

* 基本类型char(Character),boolean,byte(Byte),short(Short),int(Integer),long(Long),float(Float),double(Double)
* 基本运算符  
  instance运算符和移位运算符
* static

 静态表示的是内存的共享，就是它的每一个实例都指向同一个内存地址。把 static 拿来，就是告诉 JVM 它是静态的，它的引用（含间接引用）都是指向同一个位置  
   instance 属性在创建实例的时候初始化，static属性在类加载，也就是第一次用到这个类的时候初始化，对于后来的实例的创建，不再次进行初始化。
   

*  string  
字符串是一个特殊包装类,其引用是存放在栈里的,而对象内容必须根据创建方式不同定(常量池和堆).有的是编译期就已经创建好，存放在字符串常 量池中，而有的是运行时才被创建.使用new关键字，存放在堆中。

* 数据类型转换

1、包装类过渡类型转换  
首先声明一个变量，然后生成一个对应的包装类。如：
float f1=100.00f;
Float F1=new Float(f1);
double d1=F1.doubleValue();//F1.doubleValue()为Float类的返回double值型的方法
简单类型的变量转换为相应的包装类，可以利用包装类的构造函数。即：Boolean(boolean value)、Character(char value)、Integer(int value)、Long(long value)、Float(float value)、Double(double value)

而在各个包装类中，总有形为××Value()的方法，来得到其对应的简单类型数据。利用这种方法，也可以实现不同数值型变量间的转换，例如，对于一个双精度实型类，intValue()可以得到其对应的整型变量，而doubleValue()可以得到其对应的双精度实型变量。

2、字符串与其它类型间的转换

其它类型向字符串的转换

 调用类的串转换方法:X.toString();
 自动转换:X+"";
 使用String的方法:String.valueOf(X);
3、字符串作为值,向其它类型的转换

1、先转换成相应的封装器实例,再调用对应的方法转换成其它类型

例如，字符中"32.1"转换double型的值的格式为:new Float("32.1").doubleValue()。也可以用:Double.valueOf("32.1").doubleValue()

2、静态parseXXX方法

String s = "1";
byte b = Byte.parseByte( s );
short t = Short.parseShort( s );
int i = Integer.parseInt( s );
long l = Long.parseLong( s );
Float f = Float.parseFloat( s );
Double d = Double.parseDouble( s );
3、Character的getNumericValue(char ch)方法

4、Date类与其它数据类型的相互转换

整型和Date类之间并不存在直接的对应关系，只是你可以使用int型为分别表示年、月、日、时、分、秒，这样就在两者之间建立了一个对应关系，在作这种转换时，你可以使用Date类构造函数的三种形式：

 Date(int year, int month, int date)：以int型表示年、月、日
 Date(int year, int month, int date, int hrs, int min)：以int型表示年、月、日、时、分
 Date(int year, int month, int date, int hrs, int min, int sec)：以int型表示年、月、日、时、分、秒
在长整型和Date类之间有一个很有趣的对应关系，就是将一个时间表示为距离格林尼治标准时间1970年1月1日0时0分0秒的毫秒数。对于这种对应关系，Date类也有其相应的构造函数：Date(long date)。

获取Date类中的年、月、日、时、分、秒以及星期你可以使用Date类的getYear()、getMonth()、getDate()、getHours()、getMinutes()、getSeconds()、getDay()方法，你也可以将其理解为将Date类转换成int。

而Date类的getTime()方法可以得到我们前面所说的一个时间对应的长整型数，与包装类一样，Date类也有一个toString()方法可以将其转换为String类。

有时我们希望得到Date的特定格式，例如20020324，我们可以使用以下方法，首先在文件开始引入：

import java.text.SimpleDateFormat;
import java.util.*;

java.util.Date date = new java.util.Date();
//如果希望得到YYYYMMDD的格式
SimpleDateFormat sy1=new SimpleDateFormat("yyyyMMDD");
String dateFormat=sy1.format(date);
//如果希望分开得到年，月，日
SimpleDateFormat sy=new SimpleDateFormat("yyyy");
SimpleDateFormat sm=new SimpleDateFormat("MM");
SimpleDateFormat sd=new SimpleDateFormat("dd");
String syear=sy.format(date);
String smon=sm.format(date);
String sday=sd.format(date);
总结：

1、只有 boolean 不参与数据类型的转换

2、自动类型的转换：

 a.常数在表数范围内是能够自动类型转换的
 b.数据范围小的能够自动数据类型大的转换（注意特例）
 float 到 int，float 到 long，double 到 int，double 到 long 等由浮点类型转换成整数类型时，是不会自动转换的，不然将会丢失精度。
 c.引用类型能够自动转换为父类的
 d.基本类型和它们包装类型是能够互相转换的
3、强制类型转换：用圆括号括起来目标类型，置于变量前
 
   
  

