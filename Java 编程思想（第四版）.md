# 第一章 对象导论

## 1.1 抽象过程

> 面向对象思想实质：程序可以通过添加新类型的对象使自身适用于某个特定问题。

Alan Kay 总结面向对象语言的五个基本特性：

1. 万物皆对象。
2. 程序是对象的集合，他们通过发送消息来告知彼此所要做的。
3. 每个对象都有自己的由其他对象所构成的存储。
4. 每个对象都拥有其类型。
5. 某一特定类型的所有对象都可以接受同样的消息。

> Booch 对对象描述：**对象具有状态、行为和标识**。这意味着每个对象都可以拥有内部数据（他们给出了该对象的状态）和方法（它们产生行为），并且每一个对象都可以唯一地与其他对象区分开来，具体来说就是**每一个对象在内存中都有一个唯一的地址**。

## 1.2 每个对象都有一个接口

## 1.3 每个对象都提供服务

## 1.4 被隐藏的具体实现

Java 用三个关键字在类的内部设定边界：public、private、protected。这些**访问指定词**（access specifier）决定了紧跟其后被定义的定西可以被谁使用。

public：表示紧随其后的元素对任何人都是可用的。

private：表示除类型创建者和类型的内部方法之外的任何人都不能访问。

protected：与 private 作用相当，差别仅在于继承的类可以访问。

默认访问权限（没有使用任何访问指定词）：类可以访问在同一个包（库构造）中的其他类的成员。

## 1.5 复用具体实现

新的类可以由任意数量、任意类型的其他对象以任意可以实现新的类中想要的功能的方式所组成。因为是在使用现有的类合成新类，所以这种概念被称为**组合**（composition），如果组合式动态发生的，那么它通常被称为**聚合**（aggregation）。组合经常被视为“has-a”（拥有）关系。

## 1.6 继承

### 1.6.1 "是一个"与“像是一个”关系

## 1.7 伴随多态的可互换对象

如果不需要知道哪一段代码会被执行，那么当添加新的子类时，不需要更改调用它的方法，他就能够执行不同的代码。因此，编译器无法精确地了解哪一段代码会被执行，那么它该怎么办？

编译器不可能产生传统意义上的函数调用。一个非面向对象编程的编译器产生的函数调用会一起所谓的**前期绑定**。这么做意味着编译器将产生对一个具体函数名字的调用，而运行时将这个调用解析到将要被执行的代码的绝对地址。然而在 OOP 中，程序直到运行时才能够确定代码的地址，所以当消息发送到一个泛化对象时，必须采用其他的机制。

为了解决这个问题，面向对象程序设计语言使用了**后期绑定**的概念。当对象发送消息时，被调用的代码直到运行时才能确定。编译器确保被调用方法的存在，并对调用参数和返回值执行类型检查（无法提供此类保证的语言被称为时弱类型的），但是并不知道被执行的确切代码。

为了执行后期绑定，Java 使用一小段特殊的代码来替代绝对地址调用。这段代码使用在对象存储的信息来计算方法体的地址（这个过程将在第 8 章中详述）。这样，根据这一小段代码的内容，每一个对象都可以具有不同的行为表现。当向一个对象发送消息时，该对象就能够知道对这条消息应该做些什么。

把将导出类看做是它的基类的过程称为向上转型（upcasting）。转型（cast）这个名称的灵感来自于模型铸造的塑模动作；而向上（up）这个词来源于继承图的典型布局方式：通常基类在顶部，而导出类在其下部散开。因此，转型为一个基类就是在继承图中向上移动，即“向上转型”。

## 1.8 单根继承结构

在 Java 中，所有的类最终都继承自单一的基类（Object）。

在根单继承结构中的所有对象都具有一个公用接口，所以他们归根到底都是相同的基本类型。

单根继承结构保证所有对象都具备某些功能。可以知道在系统中可以在每个对象上执行某些基本操作。所有对象都可以很容易在堆上创建，而参数传递也得到了极大地简化。

单根继承结构使垃圾回收机器的实现变得容易得多，而垃圾回收器正是 Java 相对 C++ 的重要改进之一。由于所有对象都能够保证具有其类型信息，因此不会因为无法确定对象的类型而陷入僵局。这对于系统级操作（如异常处理）显得尤其重要，并且给编程带来了更大的灵活性。

## 1.9 容器

如果不知道在解决某个特定问题时需要多少个对象，或者他们将存活多久，那么就不可能知道如何存储这些对象。如何才能知道需要多少空间来创建这些对象？答案是不可能知道，因为这类信息只有在运行时才能获得。

对于面向对象设计中的大多数问题而言，这个问题的解决方案似乎过于轻率：创建另一种对象类型，这种新的对象类型持有对其他对象的引用。当然，你可以用在大多数语言中都有的数组类型来实现相同功能。但是这个通常被称为容器的新对象，在任何需要时都可以扩充自己以容纳你置于其中的所有东西。因此不需要知道将来会把多少个对象置于容器中，只需要创建一个容器对象，然后让他处理所有细节。

从设计的观点来看，真正需要的只是一个可以被操作，从而解决问题的序列。如果单一类型的容器可以满足所有需要，那么就没有理由设计不同类的序列了。然而还是需要对容器有所选择，这有两个原因。第一，不同容器提供了不同类型的接口和外部行为。堆栈相比于队列就具备不同的接口和行为，也不同于集合和列表的接口和行为。第二，不同容器对于某些操作具有不同的效率。ArrayList 查询快、增删满；LinkedList 增删快、查询慢。

### 1.9.1 参数化类型

在 Java SE5 出现之前，容器存储的对象都只具有 Java 中的通用类型 Object。单根继承结构，使得容器容易被复用。

由于容器只存储 Object，所以当将对象引用置入容器时，他必须被向上转型为 Object，因此它会丢失其身份。当把它取回时，向下转型为更具体的类型。除非确切知道所要处理的对象类型，否则向下转型几乎是不安全的。如果向下转型为错误类型，会得到被称为异常的运行时错误。当从容器中去除对象引用时，还是必须药以某种方式记住这些对象研究是什么类型，这样才能执行正确的向下转型。

向下转型和运行时检查需要额外的程序运行时间，创建这样的容器，它知道自己所保存的对象的类型，从而不需要向下转型以及消除犯错组的可能。这种解决方案被称为参数化类型机制。参数化类型就是一个编译器可以自动定制作用于特定类型上的类。

Java SE5 的重大变化之一就是增加了参数化类型，在 Java 中称为泛型。一堆尖括号，中间包含类型信息。

```java
ArrayList<Shape> shapes = new ArrayList<Shape>();
```

## 1.10 对象的创建和生命期

## 1.11 异常处理：处理错误

## 1.12 并发编程

## 1.13 Java 与 Internet

### 1.13.1 Web 是什么

1. 客户/服务器计算技术

客户/服务器系统的核心思想是：系统具有一个中央信息存储池（central repository of information），用来存储某种数据，它通常存在于数据库中，你可以根据需要将它分发给某些人员或机器集群。

2. Web 就是一台巨型服务器

### 1.31.2 客户端编程

1. 插件
2. 脚本语言
3. Java
4. 备选方案
5. .NET 和 C#
6. Internet 与 Intranet

### 1.13.3 服务器端编程

## 1.14 总结

# 第二章 一切都是对象

## 2.1 用引用操纵对象

## 2.2 必须由你创建所有对象

### 2.2.1 存储到什么地方

1. **寄存器**。位于处理器内部，速度最快，数量有限，按需分配，不能直接控制。
2. **堆栈**。位于通用 RAM 中，通过堆栈指针可以从处理器那里获得直接支持。堆栈指针向下异动，则分配新内存；向上移动，则释放内存。高效的分配存储方式，仅次于寄存器。
3. **堆**。通用内存池，也位于 RAM 区，用于存放所有的 Java 对象。
4. **常量存储**。常量值通常直接存放在程序代码内部，有时存放在 ROM 中（例：字符串池）。
5. **非 RAM 存储**。流对象和持久化对象。把对象转化成可以存放在其他媒介上的事物，在需要时，可以恢复成常规的、基于 RAM 的对象。

### 2.2.2 特例：基本类型

new 出来的对象存储在堆里。基本类型不用 new 创建，非引用自动变量，存放在栈里。

![image-20201228102913059](img/Java 编程思想（第四版）.assets/基本数据类型.png)

#### 高精度数字

两个高精度计算的类：BigInterger 和 BigDecimal。

BigInteger 支持任意精度的整数。

BigDecimal 支持任意精度的定点数。

### 2.2.3 Java 中的数组

在 C 和 C++ 中的数组就是内存块。如果一个程序要访问其自身内存块之外的数组，或在数组初始化之前使用内存，都会产生难以预料的后果。

Java 确保数组会被初始化，而且不能在它的范围之外被访问。这种范围检查，是以没个数组上少量的内存开销及运行时的下标检查为代价的。但换来的是安全性和效率的提高，因此付出的代价是值得的。

创建一个数组对象时，实际上就是创建了一个引用数组，并且每个引用都会自动被初始化为一个特定值，该值拥有自己的关键字 null。一旦 Java 看到 null，就知道这个引用还没有指向某个对象。在使用任何引用前，必须为其指定一个对象；如果试图使用一个还是 null 的引用，在运行时将会报错。

还可以创建用来存放基本数据类型的数组。编译器会确保这种数组的初始化，将这种属组所占的内存全部置零。

## 2.3 永远不需要销毁对象

### 2.3.1 作用域

**作用域**（scope）决定了在其内定义的变量名的可见性和生命周期。在 C、C++ 和 Java 中，作用域由花括号的位置决定。

### 2.3.2 对象的作用域

Java 对象不具备和基本类型一样的生命周期。当用 new 创建一个 Java 对象时，它可以存活于作用域之外。

```java
{
	String s = new String("a string")；
}
```

引用 s 在作用域充电就消失了。然而，s 指向的 String 对象仍继续占据内存空间。在这一小段代码中，我们无法在这个作用域之后访问这个对象，因为对它唯一的引用已超出了作用于的范围。

## 2.4 创建新的数据类型：类

class 关键字

### 2.4.1 字段和方法

定义了一个类，就可以在类中设置两种类型的元素：字段（数据成员）和方法（成员函数）。字段可以是任何类型的对象，可以通过其引用与其进行通信；也可以是基本类型中的一种。如果字段式对某个对象的引用，必须初始化改引用，使其与懿哥实际的对象相关联。

每个对象都有用来存储其字段的空间；普通字段不能在对象间共享。

引用一个对象的成员：在对象引用的名称之后紧接着一个句点，然后再接着是对象内部的成员名称。

#### 基本成员默认值

若类的某个成员是基本数据类型，即使没有初始化， Java 也会确保它获得一个默认值。

当变量作为类的成员使用时，Java 才会确保给定其默认值，局部变量并不适用。

## 2.5 方法、参数和返回值

方法的基本组成部分包括：名称、参数、返回值和方法体。

### 2.5.1 参数列表

## 2.6 构建一个 Java 程序

### 2.6.1 名字可见性

### 2.6.2 运用其他构件

想使用预先定义好的类，通过 import 关键字告诉编译器位置

### 2.6.3 static 关键字

执行 new 创建对象时，数据存储空间才被分配，其方法才供外界调用。

两种情形 new 无法解决，通过 static 可以：

1. 只想为某特定域分配单一存储空间，而不去考虑究竟要创建多少个对象，甚至跟不就不创建任何对象。
2. 希望某个方法不与包含它的类的任何对象关联在一起。即使没有创建对象，也能够调用这个方法。

示例：

```java
class StaticTest{
	static int i = 47;
}
```

## 2.7 你的第一个 Java 程序

### 2.7.1 编译和运行

指令：

```bash
javac hello.java
java hello
```

## 2.8 注释和嵌入式文档

两种注释风格：

1. 传统 /**/
2. 单行注释 //

### 2.8.1 注释文档

javadoc

javadoc 只能为 public 和 protected 成员惊醒文档注释。

### 2.8.2 语法

```java
/**
* @
* @
*/
```

### 2.8.3 嵌入式 HTML

略

### 2.8.4 一些标签示例

略

### 2.8.5 文档示例

略

## 2.9 编码风格

类名的首字母要大写；如果类名由几个单词构成，那么把他们并列在一起（不要用下划线分割名字），其中每个内部单词的首字母都采用大写形式。

这种风格称作“驼峰风格”。

方法，字段以及对象一用名称，第一个字母采用小写。

## 2.10 总结

## 2.11 练习

# 第三章 操作符

## 3.1 更简单的打印语句

## 3.2 使用 Java 操作符

## 3.3 优先级

## 3.4 赋值

赋值是使用操作符“=”。它的意思是右值复制给左值。右值可以是任何常数、变量或者表达式。但左值必须是一个明确的、已命名的变量。

举例：

```java
a = 4; //正确
// 4 = a; 错误
```

对基本数据类型赋值是很简答的。

对对象赋值的时候，真正操作的是对对象的引用。所以“将一个对象赋值给另一个对象”，实际上是将“引用”从一个地方复制到另一个地方。这意味着假若对对象使用 c=d，那么 c 和 d 都指向原本只有 d 指向的那个对象。

### 3.4.1 方法调用中的别名问题

## 3.5 算数操作符

- 自行回忆 +、-、\*、/、%、+=、-=、\*=、/=、%= 等

- 随机数，Random 类，nextInt()、nextFloat()、nextLong()、nextDouble()方法。

### 3.5.1 一元加、减操作符

负数运算加上括号，方便人看

## 3.6 自动递增和递减

自行回忆：a++、++a 等

前缀式先执行运算，再生成值；后缀式先生成值，再执行运算。

## 3.7 关系操作符

关系操作符：>、<、==、>=、<=。

关系操作符生成一个 boolean 结果，true 或者 false。

### 3.7.1 测试对象的等价性

比较基本类型使用 == 和 !=。

比较对象使用 equals()，需要自己重写该方法。

## 3.8 逻辑操作符

与（&&）或（||）非（!）根据逻辑关系，生成一个布尔值。

### 3.8.1 短路

一但能够明确无误地确定整个表达式的值，就不再计算表达式剩余部分了。（就是有一个错的就不再计算后面的了。）

## 3.9 直接常量

常数后面加后缀标志它的类型：

L 或 l 代表 long，F 或 f 代表 float，D 或 d 代表 double。

前缀符号表示不同进制：

0X 或 0x 表示十六进制，0 表示八进制。

### 3.9.1 指数计数法

在科学与工程领域，“e” 代表自然数对数基数，约等于 2.718，$1.39*e^{-43}$ 这样的表达式意味着 $1.39*2.718^{-43}$ 

但是，在 ROETRAN 语言中，设计师决定 e 代表 “10 的幂次”。C、C++、Java 中被保留了下来。Java 中 $1.39e^{-43}$ 含义是 $1.39*10^{-43}$  

## 3.10 按位操作符

按位操作符用来操作证书基本数据类型中的单个比特（bit），即二进制位。

按位与（&）、按位或（|）、按位异或（^）、按位非（~）。

## 3.11 移位操作符

移位操作符的运算对象也是二进制的位。

左移操作符（<<）：按照操作符右侧指定的位数将操作符左边的操作数向左移动（在低位补0）。

“有符号”右移位操作符（>>）：按照操作符右侧指定的位数将操作符左边的操作数向右移动。“有符号”右移位操作符使用“符号扩展”：若符号为正，则在高位插入0；若符号为负，则在高位插入1。

“无符号”右移位操作符（>>>）（Java特有），使用“零扩展：”无论正负，都在高位插入0。

如果对 char、byte 或者 short 类型的数值进行位移处理，在移位进行之前，会被转换为 int 类型，并且得到的结果也是一个 int 类型的值。只有数值右端的低 5 位才有用。（这样可以防止位移超过 int 型值所具有的位数。（译注：因为 2 的 5 次方为 32，而 int 型值只有 32 位。））**我不知道这句话在说什么，int 类型是 4 字节所以才 32 位。跟 5 有啥关系？？？**

移位可以与等号组合使用。

## 3.12 三元操作符 if-else

三元操作符也成为条件操作符，三个操作数，最终产生一个值。

表达式：boolean-exp ? value0 : value1

如果 boolean-exp 结果为 true，就计算 value0，这个计算结果就是操作符最终产生的值；如果 boolean-exp 结果为 false，就计算 value1，这个计算结果就是操作符最终产生的值。

## 3.13 字符串操作符 + 和 +=

连接不同的字符串。

如果表达式以一个字符串起头，后续所有操作数都必须是字符串型（编译器会把引号内的字符序列自动转换成字符串）。

## 3.14 使用操作符时常犯的错误

= 和 ==

## 3.15 类型转换操作符

类型转换（cast）的原意是“模型铸造”。

将希望得到的数据类型置于元括号内，放在要进行类型转换的值的左边。

```java
long lng = (long)200;
```

### 3.15.1 截尾和舍入

将 float 或者 double 转型为整型时，总对该数字执行劫尾。如果想到的到舍入的结果，需要使用 java.lang.Math 中的round() 方法。

### 3.15.2 提升

通常，表达式中出现的最大的数据类型决定了表达最终结果的数据类型。

## 3.16 Java 没有 sizeof

Java 所有数据类型在所有机器中的大小都是相同的。

## 3.17 操作符小结

在 char、byte 和 short 中，热火和一个算术运算，都会获得一个 int 结果，必须将其显式地类型转换回原来的类型（窄转换可能会造成信息的丢失），以将值赋给原本的类型。

## 3.18 总结

# 第四章 控制执行流程

## 4.1 true 和 false

所有条件语句都利用条件表达式的真或者假来决定执行路径。

Java 中不能 if(a)，需要转换为 if(a!=0)。

## 4.2 if-else

- if/else，if/else if，else
- 带不带大括号

## 4.3 迭代

while、do-while 和 for 用来控制循环，有时将他们划分为迭代语句（iteration statement）。语句会重复执行，直达起控制作用的布尔表达式（Booleanexpression）得到“假”的结果为止。

### 4.3.1 do-while

do-while 中的语句至少会执行一次。

### 4.3.2 for

第一次迭代之前要进行初始化，随后进行条件测试，每次迭代结束时，会进行某种形式的“步进”。

格式：

```java
for(initialization; Boolean-expression; step)
    statement
```

初始化（initialization）表达式、布尔表达式（Boolean-expression）、步进（step）运算，都可以为空。

### 4.3.3 逗号操作符

注意不是逗号分隔符，都好用作分隔符时用来分割函数的不同参数。Java 里唯一用到逗号操作符的地方就是 for 循环的控制表达式。在控制表达式的初始化和步进控制部分，可以使用一系列由逗号分隔的语句，而且那些语句均会独立执行。

## 4.4 Foreach 语法

录入代码时可以节省时间，易于阅读。

```java
for(float x : f)
```

## 4.5 return

return 关键词由两方面的用途：一方面指定一个方法返回什么值（假设它没有 void 返回值），另一方面他会导致当前的方法退出，并返回那个值。

如果在返回 void 的方法中没有 return 语句，那么在该方法的结尾会有一个隐式的 return，因此在方法中并非总是必须要有一个 return 语句。但是，如果一个方法声明它将返回 void 之外的其他东西，那么必须确保每一条代码路都将返回一个值。

## 4.6 break 和 continue

在任何迭代语句的主体部分，都可以用 break 和 continue 控制循环流程。其中，break 用于强行退出循环，不执行循环中剩余的语句。而 continue 则停止执行当前的迭代，然后退回循环起始处，开始下一次迭代。

## 4.7 臭名昭著的 goto

goto 起源于汇编语言的程序控制：若条件 A 成立，则跳到这里；否认则跳到那里。

goto 是 Java 中的一个保留字，但在语言中并未使用；Java 没有 goto。

Java 使用标签机制，标签是后面跟有冒号的标识符，例如：

Label1：

在 Java 中，标签起作用的唯一的地方刚好是在迭代语句之前。“刚好之前”的意思表明，在标签和迭代之间置入任何语句都不好。而在迭代之前设置标签的唯一理由是：我们希望在其中嵌套另一个迭代或者一个开关。这是由于 break 和 continue 关键词通常只中断当前循环，若随同标签一起使用，他们就会终端循环，指导标签所在的地方。

一般规则：

1. 一般的 continue 会退回最内层循环的开头（顶部），并继续执行。
2. 带标签的 continue 会到达标签的位置，并重新进入进阶在哪个标签后面的循环。
3. 一般的 break 会中断并跳出当前循环。
4. 带标签的 break 会中断并跳出标签所指的循环。

要记住的重点：Java 中需要使用标签的唯一理由就是因为由循环嵌套的存在，而且想从多层嵌套中 break 或 continue。

## 4.8 switch

switch 有时呗划归为一种选择语句。根据整数表达式的值，switch 语句可以匆匆一系列代码中选出一段去执行。格式如下：

```java
switch(integral-selector){
    case integral-value1 : statement; break;
    case integral-value2 : statement; break;
    case integral-value3 : statement; break;
    //...
    default: statement;
}
```

其中，Integral-selector（整数选择因子）是一个能够产生整数值的表达式，switch 能将这个表达式的结果与每一个 integral-value（整数值）相比较。若发现相符的，就执行对应的语句（单一的语句或多条语句，其中并不需要括号）。若没有发现相符的，就执行 default（默认）语句。

break 可选，若省略 break，会继续执行后面的 case 语句，指导遇到一个 break 为止。

选择银子必须是 int 或者 char 的整数值，否则选择因子不会工作。对于非整数类型，必须使用一系列 if 语句。

## 4.9 总结

# 第五章 初始化与清理

## 5.1 用构造器确保初始化

构造器命名：与类相同的名称。

在创建对象时，会为对象分配存储空间，并调用相应的构造器，确保了在操作对象之前，它已经被恰当地初始化了。

无参构造器叫做默认构造器。

可以创建有参数的构造器。

## 5.2 方法重载

构造器是强制重载方法名的一个原因。为了让方法名相同而形式参数不同的构造器同时存在，必须用到方法重载。

### 5.2.1 区分重载方法

区分重载方法：每个重载的方法都必须有一个独一无二的参数类型列表。

### 5.2.2 设计基本类型的重载

如果传入的数据类型（实际参数类型）小于方法中生命的参数类型，实际数据类型会被提升。

如果传入的实际参数较大，就得通过类型转换来执行窄化转换。如果不这样做，编译器就会报错。

### 5.2.3 已返回值区分重载方法

有时想要的是方法调用的其他效果（这常被称为“为了副作用而调用”），这时会调用方法而忽略其返回值。比如：

```java
f();
```

根据方法的返回值区分重载方法是行不通的。

## 5.3 默认构造器

默认构造器又名无参构造器，作用是创建一个默认对象。如果类中没有构造器，则编译器会自动创建一个默认构造器。如果已经定义了一个构造器（无论是否有参），编译器就不会自动创建构造器。

## 5.4 this 关键字

如果只有一个方法，如何指导是被 a 还是 b 调用的？

```java
class Banana(void peel(int i)){/*...*/}

public class BananaPeel{
	public static void main(String[] args){
    	Banana a = new Banana(), b = new Banana();
        a.peel(1);
        b.peel(2);
    }
}
```

编译器做了一些幕后工作。暗自把“所操作对象的引用”作为第一个参数传递给 peel()；

```java
//内部表示形式，这么写无法通过编译
Banana.peel(a, 1);
Banana.peel(b, 2);
```

只有当需要明确指出当前对象的引用时，才需要使用 this 关键字：

```java
public class Apricot{
	void pick(){/*...*/}
    void pit(){
    	pick();// 没必要 this.pick()
        /*...*/
    }
}
public class Leaf{
	int i = 0;
    Leaf increment(){
    	i++;
        return this;
    }
}
```

this 关键字对于将当前对象传递给其他方法也很有用：

```java
class Person{
	public void eat(Apple apple){
    	Apple peeled = apple.getPeeled();
        System.out.println("Yummy");
    }
}
class Peeler{
	static Apple peel(Apple apple){
    	//...remove peeel
        return apple; //Peeled
    }
}
class Apple(){
	Apple getPeeled(){
    	return Peeler.peel(this);
    }
}
public class PassingThis{
	public static void main(String[] args){
    	new Person().eat(new Apple());
    }
}
```

### 5.4.1 在构造器中调用构造器

在构造器中，如果 this 添加了参数列表，那么就有了不同的含义。这将产生对符合此参数列表的某个构造器的明确调用。

尽管可以用 this 调用一个构造器，但不能调用两个。此外，必须将构造器调用置于最起始处，否则编译器会报错。

this 的另一种用法是参数名称和成员名称相同的时候。

除构造器外，编译器禁止在其他任何方法中调用构造器。

### 5.4.2 static 的含义

static 方法就是没有 this 的方法。在 static 方法的内部不能调用非静态方法，反过来可以。Java 中禁止使用全局方法，但在类中置入 static 方法就可以访问其他 static 方法和 static 域。

## 5.5 清理：终结处理和垃圾回收

finalize() 方法。

工作原理“假定”：一旦垃圾回收器准备好释放对象占用的存储空间，将调用其 finalize() 方法，并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。

和 C++ 对比：在 C++ 中，对象一定会被销毁（如果程序中没有缺陷）；在 Java 里的对象却并非总是被垃圾回收：

1. 对象可能不被垃圾回收
2. 垃圾回收并不等于“析构”

### 5.5.1 finalize() 用途何在

不该将 finalize() 作为通用的清理方法。真正用途：

3. 垃圾回收只与内存有关

使用垃圾回收器的唯一原因是为了回收程序不再使用内存。所以对于与垃圾回收有关的任何行为，他们也必须同内存及其回收有关。

对 finalize() 的需求限制：级通过某种创建对象方式以外的方式为对象分配了存储空间。例如使用“本地方法”，本地方法是一种在 Java 中用非 Java 代码的方式。

### 5.5.2 你必须实施清理

可以肤浅地认为，正是由于垃圾收集机制存在，使得 Java 没有析构函数。垃圾回收器的存在并不能完全替代析构函数。如果希望进行除释放存储空间之外的清理工作，还得明确调用某个恰当的 Java 方法。这就等于是用析构函数了，只是没有它方便。

### 5.5.3 终结条件

当某个对象可以被清理了，这个对象应该处于某种状态，使它占用的内存可以被安全地释放。

### 5.5.4 垃圾回收器如何工作

垃圾回收器对与提高对象的创建速度具有非常明显的效果。

引用计数技术：每个对象都含有一个引用计数器，当有引用连接至对象时，引用计数加 1；当引用离开作用域或被置为 null 时，引用计数减 1。

管理引用计数开销不大，但这项开销在整个程序生命周期中将持续发生。垃圾回收器会在含有全部对象的列表上遍历，当发现某个对象的引用计数为 0 时，就释放七章勇的空间（但是，引用计数模式经常会在计数值变为 0 时立即释放对象）。

引用计数缺陷：循环引用，对象应该被回收，但引用计数却不为零。对于垃圾回收器而言，定位这样的交互自引用的对象组所需的工作量极大。

引用计数常用来说明垃圾收集的工作方式，但似乎从未被应用于任何一种 Java 虚拟机实现中。

**省略一部分不太好归纳的，细节看书**

自适应的垃圾回收技术：

1. 停止-赋值（stop-and-copy）
2. 标记-清扫（mark-and-sweep）

## 5.6 成员初始化

Java 尽力保证所有变量在使用前都能得到恰当的初始化。对于方法的局部变量，Java 以编译时错误的形式来贯彻这种保证。

类中的数据成员是基本类型，不初始化会有默认值；类里定义一个对象引用时，不初始化会得到 null。

### 5.6.1 指定初始化

## 5.7 构造器初始化

无法阻止自动初始化的进行，它将在构造器被调用之前发生。

```java
// i 会先被置为 0，然后变成 7
public class Counter{
	int i;
    Counter(){ i = 7; }
}
```



### 5.7.1 初始化顺序

在类的内部，变量定义的先后顺序决定了初始化的顺序。即使变量定义散布于方法定义之间，他们仍旧会在任何方法（包括构造器）被调用之前得到初始化。

变量->方法（包括构造器）

```java
class Window {
	Window(int marker){ print("Window(" + marker + ")");};
}
class House{
	Window w1 = new Window(1);
    House(){
    	print("House()");
        w3 = new Window(33);
    }
    Window w2 = new Window(2);
    void f(){print("f()")}
    Window w3 = new Window(3);
}
public class OrderOfIniialization{
	public static void main(String[] args){
    	House h = new House();
        h.f();
    }
}
/* Output
Window(1)
Window(2)
Window(3)
House()
Window(33)
f()
*/
```

### 5.7.2 静态数据的初始化

无论创建多少个对象，静态数据都只占用一份存储区域。

```java
class Bowl {
  Bowl(int marker) {
    print("Bowl(" + marker + ")");
  }
  void f1(int marker) {
    print("f1(" + marker + ")");
  }
}

class Table {
  static Bowl bowl1 = new Bowl(1);
  Table() {
    print("Table()");
    bowl2.f1(1);
  }
  void f2(int marker) {
    print("f2(" + marker + ")");
  }
  static Bowl bowl2 = new Bowl(2);
}

class Cupboard {
  Bowl bowl3 = new Bowl(3);
  static Bowl bowl4 = new Bowl(4);
  Cupboard() {
    print("Cupboard()");
    bowl4.f1(2);
  }
  void f3(int marker) {
    print("f3(" + marker + ")");
  }
  static Bowl bowl5 = new Bowl(5);
}

public class StaticInitialization {
  public static void main(String[] args) {
    print("Creating new Cupboard() in main");
    new Cupboard();
    print("Creating new Cupboard() in main");
    new Cupboard();
    table.f2(1);
    cupboard.f3(1);
  }
  static Table table = new Table();
  static Cupboard cupboard = new Cupboard();
} /* Output:
Bowl(1)
Bowl(2)
Table()
f1(1)
Bowl(4)
Bowl(5)
Bowl(3)
Cupboard()
f1(2)
Creating new Cupboard() in main
Bowl(3)
Cupboard()
f1(2)
Creating new Cupboard() in main
Bowl(3)
Cupboard()
f1(2)
f2(1)
f3(1)
```

### 5.7.3 显式的静态初始化

静态代码段：

```java
public class Spoon{
    satic int i;
    static{
		i = 47;
    }
}
```

这段代码只执行一次：当首次生成这个类的一个对象时，或者首次访问属于那个类的静态数据成员时（即便从未生成过那个类的对象）。

```java
class Cup {
  Cup(int marker) {
    print("Cup(" + marker + ")");
  }
  void f(int marker) {
    print("f(" + marker + ")");
  }
}

class Cups {
  static Cup cup1;
  static Cup cup2;
  static {
    cup1 = new Cup(1);
    cup2 = new Cup(2);
  }
  Cups() {
    print("Cups()");
  }
}

public class ExplicitStatic {
  public static void main(String[] args) {
    print("Inside main()");
    Cups.cup1.f(99);  // (1)
  }
  // static Cups cups1 = new Cups();  // (2)
  // static Cups cups2 = new Cups();  // (2)
} /* Output:
Inside main()
Cup(1)
Cup(2)
f(99)
*///:~
```

## 5.8 数组初始化

定义数组：

```java
//两种写法都可以
int[] a1;
int a2[];
```

编译器不允许指定数组的大小。定义的数组只是对数组的引用（该引用已经被分配了足够的存储空间），而且也没给数组对象本身分配任何空间。为了给数组创建相应的存储空间，必须写初始化表达式。

初始化：

```java
//不同的初始化方式
int a1 = {1, 2, 3, 4, 5};

int a[];
Random rand = new Random(47);
a = new int[rand.nextInt(20)];

int a[] = new int[rand.nextInt(20)];

Integer[] a = {
	new Integer(1),
    new Integer(2),
    3,
};

Integer[] b = new Integer[]{
	new Integer(1),
    new Integer(2),
    3,
}

```

后两种形式中，初始化列表的最后一个逗号都是可选的。

### 5.8.1 可变参数列表

Java SE5 中，加入可变参数列表：

```java
public class NewVarArgs {
  static void printArray(Object... args) {
    for(Object obj : args)
      System.out.print(obj + " ");
    System.out.println();
  }
  public static void main(String[] args) {
    // Can take individual elements:
    printArray(new Integer(47), new Float(3.14),
      new Double(11.11));
    printArray(47, 3.14F, 11.11);
    printArray("one", "two", "three");
    printArray(new A(), new A(), new A());
    // Or an array:
    printArray((Object[])new Integer[]{ 1, 2, 3, 4 });
    printArray(); // Empty list is OK
  }
} /* Output: (75% match)
47 3.14 11.11
47 3.14 11.11
one two three
A@1bab50a A@c3c749 A@150bd4d
1 2 3 4
*///:~
```

## 5.9 枚举类型

Java SE5 添加 enum 关键字。

简单举例：

```java
public enum Spiciness {
  NOT, MILD, MEDIUM, HOT, FLAMING
} ///:~

public class SimpleEnumUse {
  public static void main(String[] args) {
    Spiciness howHot = Spiciness.MEDIUM;
    System.out.println(howHot);
  }
} /* Output:
MEDIUM
*///:~
```

创建 enum 时，编译器会自动添加一些有用的特性。例如，创建 toString() 方法；创建 ordinal()方法，用来表示某个特定 enum 敞亮的声明顺序；创建 static values() 方法，按照 enum 常量的生命顺序，产生由这些常量构成的数组：

```java
public class EnumOrder {
  public static void main(String[] args) {
    for(Spiciness s : Spiciness.values())
      System.out.println(s + ", ordinal " + s.ordinal());
  }
} /* Output:
NOT, ordinal 0
MILD, ordinal 1
MEDIUM, ordinal 2
HOT, ordinal 3
FLAMING, ordinal 4
*///:~
```

应用于 switch：

```java
public class Burrito {
  Spiciness degree;
  public Burrito(Spiciness degree) { this.degree = degree;}
  public void describe() {
    System.out.print("This burrito is ");
    switch(degree) {
      case NOT:    System.out.println("not spicy at all.");
                   break;
      case MILD:
      case MEDIUM: System.out.println("a little hot.");
                   break;
      case HOT:
      case FLAMING:
      default:     System.out.println("maybe too hot.");
    }
  }	
  public static void main(String[] args) {
    Burrito
      plain = new Burrito(Spiciness.NOT),
      greenChile = new Burrito(Spiciness.MEDIUM),
      jalapeno = new Burrito(Spiciness.HOT);
    plain.describe();
    greenChile.describe();
    jalapeno.describe();
  }
} /* Output:
This burrito is not spicy at all.
This burrito is a little hot.
This burrito is maybe too hot.
*///:~
```

## 5.10 总结

# 第六章 访问权限控制

## 6.1 包：库单元

包内包含有一组类，他们在单一的名字空间之下被组织在了一起。

导包前：

```java
public class FullQualification {
  public static void main(String[] args) {
    java.util.ArrayList list = new java.util.ArrayList();
  }
} ///:~
```

导包后：

```java
public class SingleImport {
  public static void main(String[] args) {
    ArrayList list = new java.util.ArrayList();
  }
} ///:~
```

导入包里的所有类，举例：

```java
import java.util.*;
```

导入包是提供一个管理名字空间的机制。

当编写一个 Java 源代码文件时，此文件通常被称为编译单元（有时也被称为转译单元）。每个编译单元都必须有一个后缀名 .java，而在编译单元内则可以有一个 pubic 类，该类的名称必须与文件的名称相同（包括大小写，但不包括文件的后缀名 .java）。每个编译单元只能有一个 public 类，否则编译器就不会接受。如果在该编译单元这种还有额外的类那么在包之外的世界时无法看见这些类的，这是因为他们不是 public 类，而且他们主要用来为主 public 类提供支持。

### 6.1.1 代码组织

编译一个 .java 文件时，在 .java 文件中的每个类都会有一个输出文件，后缀为 .class。

一般编译型语言：编译器产生一个中间文件（通常是一个 obj 文件），然后再与通过链接器（用以创建一个可执行文件）或类库产生器（librarian，用以创建一个类库）产生的其他同类文件捆绑在一起。

Java：Java 可运行程序时一组可以打包并压缩为一个 Java 文档文件（JAR，使用 Java 的 jar 文档生成器）的 .class 文件。Java 解释器负责在这些文件的查找、装载和解释。

类库实际上是一组类文件。其中每个文件都有一个 public 类，以及任何数量的非 public 类。因此每个文件都有一个构件。如果希望这些构件（每一个都有他们自己的独立的 .java 和 .class 文件）属于同一个群组，可以使用关键字 package。

### 6.1.2 创建独一无二的包名

将特定包的所有 .class 文件都置于一个目录下。

将所有的文件收入一个子目录还可以解决另外两个问题：怎样创建独一无二的名称，以及怎样查找有可能隐藏于目录结构中某处的类。这些任务是通过将 .class 文件所在的路径位置编码成 package 的名称来实现的。

独一无二解决：域名唯一，按照惯例，package 名称的第一部分是类的创建者的反序的 Internet 域名。

位置确定：package 名称问劫尾机器上的一个目录。Java 程序运行并需要加载 .class 文件的时候，可以确定 .class 文件在目录上的位置。

Java 解释器的运行过程：首先，找出环境变量 CLASSPATH（可以通过操作系统来设置，有时也可通过安装程序-用来在你的机器上安装 Java 或基于 Java 的工具-来设置）。CLASSPATH 包含一个或多个目录，用做查找 .class 文件的根目录。从根目录开始，解释器获取包的名称并将每个据点替换成反斜杠，以从 CLASSPATH 根中产生一个路径名称（于是，package foo.bar.baz 就便成为 foo\bar\baz 或 foo/bar/baz 或其他，这一切取决于操作系统）。得到路径会与 CLASSPATH 中的各个不同的项相连接，解释器就在这些目录中查找与你所要创建的类名称相关的 .class 文件。（解释器还会去查找某些涉及 Java 解释器所在位置的标准目录。）

### 6.1.3 定制工具库

举例：

```java
public class Print {
  // Print with a newline:
  public static void print(Object obj) {
    System.out.println(obj);
  }
  // Print a newline by itself:
  public static void print() {
    System.out.println();
  }
  // Print with no line break:
  public static void printnb(Object obj) {
    System.out.print(obj);
  }
  // The new Java SE5 printf() (from C):
  public static PrintStream
  printf(String format, Object... args) {
    return System.out.printf(format, args);
  }
} ///:~
```

使用：

```java
public class PrintTest {
  public static void main(String[] args) {
    print("Available from now on!");
    print(100);
    print(100L);
    print(3.14159);
  }
} /* Output:
Available from now on!
100
100
3.14159
*///:~
```

### 6.1.4 用 import 改变行为

Java 没有 C 的条件编译功能。

条件编译还有其他一些有价值的用途。调试。调试功能在开发过程中是开启的，在发布的产品中是禁用的。可以通过修改被导入的 package 的方法实现这一目的，修改的方法是将程序中用到的代码从调试版改为发布版。这一种技术适用于任何种类的条件代码。

### 6.1.5 对使用包的忠告

务必记住，无论何时创建包，都已经再给定包的名称的时候隐含地指定了目录结构。这个包必须位于其名称所指定的目录之中，而该目录必须是在以 CLASSPATH 开始的目录中可以查询到的。

注意，编译过的代码通常放置在与源代码不同的目录中，但是必须保证 JVN 使用 CLASSPATH 可以找到该路径。

## 6.2 Java 访问权限修饰词

### 6.2.1 包访问权限修饰词

默认访问权限没有任何关键字，通常是指包访问权限，有时也表示成为 friendly。

取得对某成员的访问权限的唯一途径：

1. 使该成员成为 public。
2. 通过不加访问权限修饰词并将其他类放置于同一个包内。
3. 继承。继承来的类可以访问 public 成员和 protected 成员，但 private 成员不行。
4. 提供访问器（accessor）和变异器（mutator）方法（也可以称作 get/set 方法），以读取和改变数值。

### 6.2.2 public：接口访问权限

使用关键字 public，意味着 public 之后紧跟着的成员生命自己对每个人都是可用的，尤其是使用类库的客户端程序员更是如此。

### 6.2.3 private：你无法访问

private 关键字：除了包含该成员的类之外，其他任何类都无法访问这个成员。

示例：控制如何创建对象，阻止别人直接访问某个特定的构造器（或全部构造器）。

```java
class Sundae {
  private Sundae() {}
  static Sundae makeASundae() {
    return new Sundae();
  }
}

public class IceCream {
  public static void main(String[] args) {
    //! Sundae x = new Sundae();
    Sundae x = Sundae.makeASundae();
  }
} ///:~
```

### 6.2.4 protected：继承访问权限

## 6.3 接口和实现

访问权限控制常被称为是**具体实现的隐藏**。把数据和方法包装进类中，以及具体实现的隐藏，长共同被称作是**封装**。其结果是一个同时带有特征和行为的数据类型。

访问权限控制将权限的边界划在了数据类型的内部。出于两个原因：

1. 要设定客户端程序员可以使用和不可以使用的界限。可以在结构中简历自己内部机制，而不必担心客户端程序员会偶然地将内部机制当作是他们可以使用的接口的一部分。
2. 接口和具体实现进行分离。如果结构是用于一组程序之中，而客户端程序员除了可以想接口发送信息之外什么也不可以做的话，那么就可以随意更改所有不是 public 的东西（例如有包访问权限、protected 和 private 的成员），而不会破坏客户端代码。

## 6.4 类的访问权限

修饰词必须出现于关键字 class 之前。

一些限制：

1. 每个编译单元（文件）都只能有一个 public 类。
2. public 类的名称必须与含有该编译单元的文件名相匹配，包括大小写。
3. 编译单元内完全不带 public 类也是可能的。这种情况下，可以随意对文件命名。

## 6.5 总结

# 第七章 复用类

## 7.1 组合语法

只需将对象引用置于新类中即可。

类中的对象引用不手动初始化时，为它们调用任何方法都会得到“运行时错误”异常。

初始化引用的位置：

1. 在定义对象的时候。这意味着它们总能够在构造器被调用之前被初始化。
2. 在类的构造器中。
3. 就在正要使用这些对象之前，这种方式成为惰性初始化。在生成对象不值得及不必每次都生成的情况下，这种方式可以减少额外的负担。
4. 使用实例初始化

示例：

```java
//: reusing/Bath.java
// Constructor initialization with composition.
import static net.mindview.util.Print.*;

class Soap {
  private String s;
  Soap() {
    print("Soap()");
    s = "Constructed";
  }
  public String toString() { return s; }
}	

public class Bath {
  private String // Initializing at point of definition:
    s1 = "Happy",
    s2 = "Happy",
    s3, s4;
  private Soap castille;
  private int i;
  private float toy;
  public Bath() {
    print("Inside Bath()");
    s3 = "Joy";
    toy = 3.14f;
    castille = new Soap();
  }	
  // Instance initialization:
  { i = 47; }
  public String toString() {
    if(s4 == null) // Delayed initialization:
      s4 = "Joy";
    return
      "s1 = " + s1 + "\n" +
      "s2 = " + s2 + "\n" +
      "s3 = " + s3 + "\n" +
      "s4 = " + s4 + "\n" +
      "i = " + i + "\n" +
      "toy = " + toy + "\n" +
      "castille = " + castille;
  }	
  public static void main(String[] args) {
    Bath b = new Bath();
    print(b);
  }
} /* Output:
Inside Bath()
Soap()
s1 = Happy
s2 = Happy
s3 = Joy
s4 = Joy
i = 47
toy = 3.14
castille = Constructed
*///:~
```

## 7.2 继承语法

在继承过程中，需要先声明“新类与旧类相似”。这种声明时通过类在主题的左边花括号之前，书写后面紧随基类名称的关键字 extends 而实现的。当这么做时，回自动得到基类中所有的域和方法。

举例和说明略。

## 7.3 代理

Java 并没有提供对代理的直接支持。这是继承与组合之间的中庸之道，因为我们将懿哥成员对象置于所要构造的类中（就像组合），但与此同时我们在新类中暴露了该成员对象的所有方法（就像继承）。

例如：太空船需要一个控制模块：

```java
//: reusing/SpaceShipControls.java

public class SpaceShipControls {
  void up(int velocity) {}
  void down(int velocity) {}
  void left(int velocity) {}
  void right(int velocity) {}
  void forward(int velocity) {}
  void back(int velocity) {}
  void turboBoost() {}
} ///:~
```

继承的方式：

```java
//: reusing/SpaceShip.java

public class SpaceShip extends SpaceShipControls {
  private String name;
  public SpaceShip(String name) { this.name = name; }
  public String toString() { return name; }
  public static void main(String[] args) {
    SpaceShip protector = new SpaceShip("NSEA Protector");
    protector.forward(100);
  }
} ///:~
```

代理的方式：继承的方式中，SpaceShipControls 所有的方法在 SpaceShip 中暴露，代理解决了此类问题。

```java
//: reusing/SpaceShipDelegation.java

public class SpaceShipDelegation {
  private String name;
  private SpaceShipControls controls =
    new SpaceShipControls();
  public SpaceShipDelegation(String name) {
    this.name = name;
  }
  // Delegated methods:
  public void back(int velocity) {
    controls.back(velocity);
  }
  public void down(int velocity) {
    controls.down(velocity);
  }
  public void forward(int velocity) {
    controls.forward(velocity);
  }
  public void left(int velocity) {
    controls.left(velocity);
  }
  public void right(int velocity) {
    controls.right(velocity);
  }
  public void turboBoost() {
    controls.turboBoost();
  }
  public void up(int velocity) {
    controls.up(velocity);
  }
  public static void main(String[] args) {
    SpaceShipDelegation protector =
      new SpaceShipDelegation("NSEA Protector");
    protector.forward(100);
  }
} ///:~
```

## 7.4 结合使用组合和代理

### 7.4.1 确保正确清理

### 7.4.2 名称屏蔽

如果 Java 的基类拥有某个已被对此重载的方法名称，那么在到处类中重新定义该方法名称并不会屏蔽其在基类中的任何版本。因此，无论时在该层或者其他基类中对方发进行定义，重载机制都可以正常工作。

Java SE5 新增了 @Override 注解，他并不是关键字，但可以把它当作关键字使用。当想要覆盖写某个方法时，可以选在添加这个注解，在你不留心重载而并非覆盖写了该方法时，变异器就会生成一条错误消息。这样，@Override 注解可以防止你在不想重载时而意外地进行了重载。

## 7.5 在组合与继承之间选择

组合技术通常用于想在新类中使用现有类的功能而非他的接口这种情形。即，在新类中嵌入某个对象，让其实现所需要的功能，但新类的用户看到的只是为新类所定义的接口，而非嵌入对象的入口。为取得此效果，需要在新类中嵌入一个现有类的 private 对象。

有时，允许类的用户直接访问新类中的组合成分是极具意义的；也就是说将成员对象声明为 public。如果成员对象自身都隐藏了具体实现，那么这种做法是安全的。当用户能够了解到你正在组装一组部件时，回使得端口更加易于理解。

## 7.6 protected 关键字

## 7.7 向上转型

### 7.7.1 为什么成为向上转型

该属于的使用有其历史原因，并且时以传统的类继承图的绘制方法为基础：将根置于页面的顶端，然后逐渐向下。

### 7.7.2 在讨论组合与继承

组合运动的多一点，继承不太常用。应当慎用继承，其使用场景仅限于你确信使用该技术确实有效的情况。

到底是该使用组合还是继承，一个最清晰的判断办法就是我呢疑问自己是否需要从新类向基类进行向上转型。如果必须向上转型，则集成是必要的；如果不需要，则应当好好考虑自己是否需要继承。第八章提出了一个使用向上转型的最具说服力的理由，但只要记得子问一下“我真的需要向上转型吗？”就能较好地在这两种技术中作出决定。

## 7.8 final 关键字

final 通常指的是“这是无法改变的”。不想改变可能处于两种理由：设计或者效率。

可能使用 final 的三种情况：数据、方法和类。

### 7.8.1 final 数据

有时数据的恒定不变是很有用的，比如：

1. 一个永不改变的编译时常量
2. 一个在运行时被初始化的值，你不希望它被改变。

final 修饰的常量必须是基本数据类型，定义的时候必须进行赋值。final 使数值恒定不变。

对于对象引用，final 使引用恒定不变。一旦引用被初始化指向一个对象，就无法再把它改为指向另一个对象。然而，对象其自身使可以被修改的。

#### 空白 final

Java 允许生成空白 final，空白 final 指被声明为 final 但又未给定初值的域。无论什么情况，编译器都确保空白 final 在使用前必须被初始化。空白 final 在关键在 final 的使用上提供了更大的灵活性，为此，一个勒种的 final 域就可以做到根据对象而有所不同，却又保持其恒定不变的特性。

```java
//: reusing/BlankFinal.java
// "Blank" final fields.

class Poppet {
  private int i;
  Poppet(int ii) { i = ii; }
}

public class BlankFinal {
  private final int i = 0; // Initialized final
  private final int j; // Blank final
  private final Poppet p; // Blank final reference
  // Blank finals MUST be initialized in the constructor:
  public BlankFinal() {
    j = 1; // Initialize blank final
    p = new Poppet(1); // Initialize blank final reference
  }
  public BlankFinal(int x) {
    j = x; // Initialize blank final
    p = new Poppet(x); // Initialize blank final reference
  }
  public static void main(String[] args) {
    new BlankFinal();
    new BlankFinal(47);
  }
} ///:~
```

必须在域的定义出或者每个构造器中用表达式对 final 进行赋值，这正式 final 域在使用前总使被初始化的原因所在。

#### final 参数

Java 允许在参数列表中以生命的方式将参数指明为 final。这意味着你无法在方法中更改参数引用所指向的对象。

### 7.8.2 final 方法

使用 final 方法的原因有两个。第一个原因是把方法锁定，以放置任何继承类修改它的含义。这是处于设计的考虑：想要确保在继承中是方法行为保持不变，并且不会覆盖。

建议使用 final 方法的第二个原因是效率。早起 Java 实现中，如果讲一个方法指指明为 final，就是同意编译器将针对该方法的所有调用都转为内嵌调用。……省略细节，就是会快，但如果一个方法很大，效果不明显。

Java 新版本中，虚拟机进行优化，不用使用 final 方法进行优化。

#### final 和 private 关键字

类中所有的 private 方法都是隐式地指定为 final 的。由于无法取用 private 方法，所以也就无法覆盖它。可以对 private 方法添加 final 修饰词，但这并不能给该方法增加任何额外的意义。

这一问题会造成混淆。因为，如果你是图覆盖一个 private 方法（隐含是 final 的），似乎是奏效的，而且编译器也不回给出错误信息。

### 7.8.3 final 类

当将某个类的整体定义为 final 时（通过将关键字 final 置于它的定义之前），就表明了你不打算继承该类，而且允不允许别人这样做。换句话说，出于某种考虑，你对该类的设计用不需要做任何改动，或者出安全考虑，你不希望它有子类。

### 7.8.4 有关 final 的忠告

在设计类时，将方法指明是 final 的，应该说是明智的。你可能会觉得，没人会想要覆盖你的方法。有时这是对的。

但请留意你所做的假设。要遇见类是如何被服用的一般是很困难的，特别对于一个通用类而言更是如此。如果讲一个方法指定为 final，可能会妨碍其他程序员在项目中通过继承来复用你的类，而这只是因为你没有想到他会以那种方式被运用。

举例：

Java 标准库 1.0/1.1 Vector 类后来被现在 Java 的容器库用 ArrayList 替代。

Java 标准库 1.0/1.1 Hashtable 类后来被现在 Java 的容器库用 HashMap 替代。

## 7.9 初始化及类的加载

在许多传统语言中，程序师作为启动过程的一部分立即被加载的。然后说是初始化，紧接着程序开始运行。这些语言的初始化过程必须小心控制，以确保定义为 static 的东西，其初始化顺序不会造成麻烦。例如 C++ 中，如果某个 static 期望另一个 static 在被初始化之前就能有效的使用它们就会出现问题。

Java 不会出现这个问题，因为他在用了一种不同的加载方式。一般来说，可以说：“类的代码在初次使用时才加载“。这通常指加载发生于创建累的第一个对象之时，但是当访问 static 域或 static 方法时，也会发生加载。

初次使用之处也是 static 初始化发生之处。所有的 static 对象和 static 代码段都会在加载时依程序中的顺序（即，定义类时的书写顺序）而依次初始化。定义为 static 的东西只会被初始化一次。

### 7.9.1 继承与初始化

举例观察：

```java
//: reusing/Beetle.java
// The full process of initialization.
import static net.mindview.util.Print.*;

class Insect {
  private int i = 9;
  protected int j;
  Insect() {
    print("i = " + i + ", j = " + j);
    j = 39;
  }
  private static int x1 =
    printInit("static Insect.x1 initialized");
  static int printInit(String s) {
    print(s);
    return 47;
  }
}

public class Beetle extends Insect {
  private int k = printInit("Beetle.k initialized");
  public Beetle() {
    print("k = " + k);
    print("j = " + j);
  }
  private static int x2 =
    printInit("static Beetle.x2 initialized");
  public static void main(String[] args) {
    print("Beetle constructor");
    Beetle b = new Beetle();
  }
} /* Output:
static Insect.x1 initialized
static Beetle.x2 initialized
Beetle constructor
i = 9, j = 0
Beetle.k initialized
k = 47
j = 39
*///:~
```

在Beetle上运行Java时，发生的第一件事就是试图访问Beetle.main()(一个static方法），于是加载器开始启动并找出Beetle类的编译代码(在名为Beetle.class的文件之中）。在对他进行加载的过程中，编译器注意到他有一个基类(这是由关键字extends得知的)，于是它继续加载(该基类），不管你是否打算产生一个该基类的对象，这都会发生(请尝试将对象创建代码注释掉,以证明这一点)。

如果该基类还有其自身的基类，那么第二个基类会被加载，如此类推，直到根基类。接下来，根基类(在本例中时Insect)中的static初始化将被执行，然后是下一个导出类，以此类推。

至此为止，必要的类都已加载完毕，对象就可以被创建了(在本例中，Beetle beetle=new Beetle())。首先，对象中所有的基本类型都会被设为默认值，对象中的对象引用被设为null。然后，基类的构造器被调用。在基类构造器被调用完成之后，实例变量按顺序被初始化，最后，构造器的其余部分被执行。

## 7.10 总结

# 第八章 多态

## 8.1 再论向上转型

对象既可以作为自己本身的类使用，也可以作为它的基类型使用。把对某个对象的引用视为对其基类型的引用的做法被称作**向上转型**（因为在继承树的画法中，基类是放置在上方的）。

### 8.1.1 忘记对象类型

## 8.2 转机

### 8.2.1 方法调用绑定

将一个方法调用同一个方法主体关联起来被称作**绑定**。

**前期绑定**：程序执行**前期绑定**（如果有的话，由编译器和连接程序实现）。面向过程语言默认的绑定方式。

**后期绑定**：在运行时根据对象的类型进行绑定。后期绑定也叫做**动态绑定**或者**运行时绑定**。

Java 中除了 static 方法和 final 方法（private 方法 属于 final 方法）之外，其他所有的方法都是后期绑定。

### 8.2.2 产生正确的行为

### 8.2.3 可扩展性

### 8.2.4 缺陷：“覆盖”私有化

结论：只有非 private 方法才可以被覆盖；但是还需要密切注意 private 方法的现象，这时虽然编译器不会报错，但是也不会按照我们所期望的来执行。确切地说，在导出类中，对于基类中的 private 方法，最好采用不同的名字。

### 8.2.5 缺陷：域与静态方法

**只有普通方法调用可以是多态的**。

例如，如果直接访问某个域，这个访问就将在编译期进行解析：

```java
//: polymorphism/FieldAccess.java
// Direct field access is determined at compile time.

class Super {
  public int field = 0;
  public int getField() { return field; }
}

class Sub extends Super {
  public int field = 1;
  public int getField() { return field; }
  public int getSuperField() { return super.field; }
}

public class FieldAccess {
  public static void main(String[] args) {
    Super sup = new Sub(); // Upcast
    System.out.println("sup.field = " + sup.field +
      ", sup.getField() = " + sup.getField());
    Sub sub = new Sub();
    System.out.println("sub.field = " +
      sub.field + ", sub.getField() = " +
      sub.getField() +
      ", sub.getSuperField() = " +
      sub.getSuperField());
  }
} /* Output:
sup.field = 0, sup.getField() = 1
sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0
*///:~
```

当 Sub 对象转型为 Super 引用时，任何域访问操作都将由编译期解析，因此不是多态的。在本例中，为 Super.field 和 Sub.field 分配了不同的存储空间。这样，Sub 实际上包含两个被称为 fiels 的域：它自己的和它从 Super 处得到的。然而在引用 Sub 中的 field 时所产生的默认域并非 Super 版本的 field 域。因此，为了得到 Super.field，必须显式地指明 super.field。

尽管这看起来好像会成为一个容易令人混淆的问题，但是在实践中，它实际上从来不会发生。首先，你通常会将所有的域都设置成 private，因此不能直接访问他们，其副作用是只能调用方法来访问。另外，你可能不会对基类中的域和导出类中的域赋予相同的名字，因为这种做容易令人混淆。

**翻译问题，域可以理解为变量：**

============================================================

Java中的Field译为“字段”，也译为“域”，Field和成员变量（Member Variable）是相同的。所以域是变量中的一种。

关于Java中的变量，官方文档中如是说：

>  There are several kinds of variables: 

- Member variables in a class—these are called fields.
- Variables in a method or block of code—these are called local variables.
- Variables in method declarations—these are called parameters.

翻译过来即：

>  Java中有如下几种变量： 

- 类中的成员变量——称为字段（亦即 “域”）
- 一个方法或代码块中的变量——称为局部变量（亦即 “本地变量”）
- 在方法声明中的变量——称为参数

**成员变量**

包含：类变量（也称静态变量、静态域）和实例变量（也称实例域、非静态域）。

**类变量**

由static修饰，每个类的实例共享一个类变量，它位于内存中的一个固定位置。任何对象都可以改变类变量的值，但是也可以在不创建类的实例的情况下操作类变量。

**实例变量**

当一个类实例化多个对象时，它们都有自己独立的实例变量副本。每个对象都有自己的这些变量的值，存储在不同的内存位置。

============================================================

如果某个方法是静态的，它的行为就不具有多态性：

```java
//: polymorphism/StaticPolymorphism.java
// Static methods are not polymorphic.

class StaticSuper {
  public static String staticGet() {
    return "Base staticGet()";
  }
  public String dynamicGet() {
    return "Base dynamicGet()";
  }
}

class StaticSub extends StaticSuper {
  public static String staticGet() {
    return "Derived staticGet()";
  }
  public String dynamicGet() {
    return "Derived dynamicGet()";
  }
}

public class StaticPolymorphism {
  public static void main(String[] args) {
    StaticSuper sup = new StaticSub(); // Upcast
    System.out.println(sup.staticGet());
    System.out.println(sup.dynamicGet());
  }
} /* Output:
Base staticGet()
Derived dynamicGet()
*///:~

```

**静态方法是与类，而并非与单个的对象相关联的。**

## 8.3 构造器和多态

构造器并不具有多态性，他们实际上是 static 方法，只不过该 static 声明是隐式的。

### 8.3.1 构造器的调用顺序

基类的狗在其总是在导出类的构造过程中被调用，而且按照继承层次逐渐向上链接，以使每个基类的构造器都能得到调用。

这样做是有意义的，因为构造器具有一项特殊任务：检查对象是否被正确地构造。导出类只能方位它自己的成员，不能方位基类中的成员（基类成员通常是 private 类型）。只有基类的构造器才具有恰当的只是和权限来对自己的元素进行初始化。因此，必须令所有的构造器都得到调用，否则就不可能正确构造完整对象。这正是编译器为什么要强制每个导出类部分都必须调用构造器的原因。在导出类的构造器主体中，如果没有明确指定调用某个基类构造器，它就会“默默”地调用默认构造器。如果不存在默认构造器，编译器就会报错（若某个类没有构造器，编译器会自动合成出一个默认构造器）。

```java
//: polymorphism/Sandwich.java
// Order of constructor calls.
package polymorphism;
import static net.mindview.util.Print.*;

class Meal {
  Meal() { print("Meal()"); }
}

class Bread {
  Bread() { print("Bread()"); }
}

class Cheese {
  Cheese() { print("Cheese()"); }
}

class Lettuce {
  Lettuce() { print("Lettuce()"); }
}

class Lunch extends Meal {
  Lunch() { print("Lunch()"); }
}

class PortableLunch extends Lunch {
  PortableLunch() { print("PortableLunch()");}
}

public class Sandwich extends PortableLunch {
  private Bread b = new Bread();
  private Cheese c = new Cheese();
  private Lettuce l = new Lettuce();
  public Sandwich() { print("Sandwich()"); }
  public static void main(String[] args) {
    new Sandwich();
  }
} /* Output:
Meal()
Lunch()
PortableLunch()
Bread()
Cheese()
Lettuce()
Sandwich()
*///:~
```

对象调用构造器要遵照下面的顺序：

1. 调用基类构造器。这个步骤会不断地反复递归下去，首先是构造这种层次结构的根，然后是下一层导出类，等等，指导最低层的导出类。
2. 按证明顺序调用成员的初始化方法。
3. 调用导出类构造器的主体。

### 8.3.2 继承与清理

通过组合和继承方法创建新类时，子对象通常会留给垃圾回收器进行处理。

如果有特殊清理动作，就必须在导出类覆盖 dispose() 方法。覆盖被继承类的 dispose() 方法时，务必记住调用基类版本 dispose() 方法；否则，基类的清理动作不会发生。

万一某个子对象要依赖于其他对象，销毁的顺序应该和初始化顺序相反。对于字段，则意味着与声明的顺序相反（因为字段的舒适化时按照生命的顺序进行的）。对于基类（遵循 C++ 中析构函数的形式），首先对其导出类进行清理，然后才是基类。这是因为导出类的清理可能会调用基类中的某些方法，所以需要使基类中的构件仍起作用而不应过早地销毁。

### 8.3.3 构造器内部的多态方法的行为

如果在一个构造器的内部调用正在构造的对象的某个动态绑定方法，会发生什么情况？

如果构造器只是在构建对象过程中的一个步骤，并且该对象所属的类是从这个构造器所属的类导出的，那么导出部分在当前构造器正在被调用的时刻任然是没有被初始化的。

然而，一个动态绑定的方法调用却会向外深入到继承层次结构内部，他可以调用导出类里的方法。如果我们是在构造器内部这样做，那么就可能会调用某个方法，而这个方法所操纵的成员可能还未进行初始化。

```java
//: polymorphism/PolyConstructors.java
// Constructors and polymorphism
// don't produce what you might expect.
import static net.mindview.util.Print.*;

class Glyph {
  void draw() { print("Glyph.draw()"); }
  Glyph() {
    print("Glyph() before draw()");
    draw();
    print("Glyph() after draw()");
  }
}	

class RoundGlyph extends Glyph {
  private int radius = 1;
  RoundGlyph(int r) {
    radius = r;
    print("RoundGlyph.RoundGlyph(), radius = " + radius);
  }
  void draw() {
    print("RoundGlyph.draw(), radius = " + radius);
  }
}	

public class PolyConstructors {
  public static void main(String[] args) {
    new RoundGlyph(5);
  }
} /* Output:
Glyph() before draw()
RoundGlyph.draw(), radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(), radius = 5
*///:~
```

注意：第二行输出是 0，不是 1；

初始化实际过程：

1. 在其他任何食物发生之前，将分配给对象的存储空间初始化成二进制的零。
2. 如果所述那样调用基类构造器。此时，调用被覆盖后的 draw() 方法（要在调用 RoundGlyph 构造器之前调用），由于步骤 1 的缘故，我们此时会发现 radius 的值为 0。
3. 按照生命的顺序调用成员的初始化方法。
4. 调用导出类的构造器主体。

## 8.4 协变返回类型

Java SE5 中添加了斜边返回类型，他表示在导出类中的被覆盖方法可以返回基类方法的返回类型的某种导出类型：

```java
//: polymorphism/CovariantReturn.java

class Grain {
  public String toString() { return "Grain"; }
}

class Wheat extends Grain {
  public String toString() { return "Wheat"; }
}

class Mill {
  Grain process() { return new Grain(); }
}

class WheatMill extends Mill {
  Wheat process() { return new Wheat(); }
}

public class CovariantReturn {
  public static void main(String[] args) {
    Mill m = new Mill();
    Grain g = m.process();
    System.out.println(g);
    m = new WheatMill();
    g = m.process();
    System.out.println(g);
  }
} /* Output:
Grain
Wheat
*///:~
```

Java SE5 与 Java 较早版本之间的主要差异就是较早的版本将请吃 process() 的覆盖版本必须返回 Grain，而不能返回 Wheat，尽管 Wheat 是从 Grain 导出的，因而也应该是一种合法的返回类型。协变返回类型允许返回更具体的 Wheat 类型。

## 8.5 用继承进行设计

是用现成的类建立新类时，最好的方式是首选组合。组合不会轻质程序设计进入继承的层次结构。而且，组合更加灵活，可以动态选择类型；相反，集成在编译时就需要知道确切的类型。

一条通用准则：用继承表达行为间的差异，并用字段表达状态上的变化。

### 8.5.1 纯继承与扩展

纯替代关系（“is-a”）：只有在基类中已经及那里的方法才可以在导出类中被覆盖。

扩展接口（“is-like-a”）（解决特定问题的完美方案）：有相同的基本接口，但还具有额外方法实现的其他特性。

is-like-a 缺点：导出类中接口扩展部分不能被基类访问，因此，一旦向上转型，就不能调用那些新方法。

### 8.5.2 向下转型与运行时类型识别

向上转型会修饰具体的类型信息，但是安全。

向下转型不安全，需要有某种方法确保向下转型的正确性。

Java 中所有转型都会得到检查！即使是普通的加括弧形式的类型转换，在进入运行期时仍然会对其进行检查。如果不是们想要的类型就会返回一个 ClassCastException（类型转换异常）。这种在运行期间对类型进行检查的行为称作“运行时类型识别”（RTTI）

## 8.6 总结

# 第九章 接口

## 9.1 抽象类和抽象方法

抽象方法机制；仅有声明而没有方法体。

语法：

```java
abstract void f();
```

包含抽象方法的类叫做抽象类。如果一个类包含一个或多个抽象方法，该类必须被限定为抽象的。否则，编译器会报错。

创建抽象类的对象，编译器会报错，保证抽象类的纯粹性。

如果从一个抽象类继承，并想创建该新类的对象，那么就必须为基类中的所有抽象方法提供方法定义。如果不这样做（可以选择不做），那么导出类便也是抽象类，切编译器将会强制我们用 abstract 关键字来限定这个类。

## 9.2 接口

interface 这个关键字产生一个完全抽象的类，它根本就没有提供任何具体实现。它允许创建者确定方法名、参数列表和返回类型，但是没有任何方法体。接口只提供了形式，而未提供任何具体实现。

一个借口表示：“所有实现了该特定接口的类看起来都像这样”。

想要创建一个接口，需要用 interface 关键字来替代 class 关键字。可以在 interface 关键字面前添加 public 关键字（但仅限于该接口在与其同名的文件中被定义）。如果不添加 public 关键字，则它只具有包访问权限，这样他就只能在同一个包内可用。接口也可以包含域，但这些域隐式地是 static 和 final 的。

要让一个类遵循某个特定接口（或者是一组接口），需要使用 implements 关键字，他表示L“interface 只是他的外貌，但是现在我要声明它是如何工作的”。

接口中的方法是隐式地 public 的。当要实现一个接口时，在接口中定义的方法必须被定义为是 public 的；否则，他们将只能得到默认的包访问权限，这样在方法被继承的过程中，其访问权限就被降低了，这是 Java 编译器所不允许的。

## 9.3 完全解耦（暂时略过）

## 9.4 Java 中的多继承





















































# 第十五章 泛型

- 在面向对象变成语言中，多态算是一种泛化机制。例如，你可以将方法的参数类型设置为基类，那么该方法就可以接受从这个基类中导出的任何类作为参数。
- Java SE5 提出泛型概念。
- 泛型实现了 **参数化类型** 的概念，使代码可以应用于多种类型。
- 泛型术语意思：适用于许多的类型。
- 在你创建参数化类型的一个实例时，编译器会为你负责转型操作，并保证类型的正确性。

## 15.1 与 C++ 的比较

## 15.2 简单泛型

Java 泛型核心概念：告诉编译器想使用什么类型，编译器处理一切细节。

### 15.2.1 一个元组类库

元组：将一组对象直接打包存储与其中的一个单一对象。这个容器对象允许读取其中元素，但是不允许像其中存放新的对象。

```java
public class TwoTuple<A,B>{
    public final A first;
    public final B second;
    public TwoTuple(A a, B b){
        first = a;
        second = b;
    }
}
```

- 元组隐含地保持了其中元素的次序。
- final 声明保证了安全性。
- 利用继承机制实现长度更长的元组。

### 15.2.2 一个堆栈类

### 15.2.3 RandomList

## 15.3 泛型接口

- 泛型可以应用于接口。例如生成器（generator），这是一种专门负责创建对象的类。

```java
public interface Generator<T> { T next(); }
```

## 15.4 泛型方法 ？

- 是否拥有泛型方法，与其所在的类是否是泛型没有关系。
- 泛型方法使得该方法能够独立于类而产生变化。

> 基本指导原则：无论何时，只要能做到，就应该只使用泛型方法。
>
> 也就是说，如果使用泛型方法可以取代整个类泛型化，那么就应该只使用泛型方法，因为它可以使事情更清楚明白。
>
> 对于一个 static 的方法而言，无法访问泛型类的类型参数，所以，如果 static 方法需要使用泛型的能力，就必须使其成为泛型方法。

定义泛型方法：将泛型参数列表置于返回值之前。

```java
public class GenericMenthods{
    public <T> void f(T x){
    	System.out.println(x.getClass().getName());
	}
    public static void main(String[] args){
        GenericMethods gm = new GenericMethods();
        gm.f("");
        gm.f(1);
    }
}
```

> 当时用**泛型类**时，必须在创建对象的时候指定类型参数的值，而使用**泛型方法**的时候，通常不必指明参数类型，因为编译器会为我们找出具体的类型。这称为**类型参数推断**。
>

### 15.4.1 杠杆利用类型参数推断

- 类型推断只对赋值操作有效。

> 如果将一个泛型方法调用的结果（例如New.map()）作为参数，传递给另一个方法，这时编译器并不会执行类型推断。在这种情况下，编译器认为：调用泛型方法后，其返回值被赋给一个 Object 类型的变量。
>

- 显式的类型说明（编写非赋值语句时使用）

> 要显式地指明类型，必须在点操作符与方法名之间插入尖括号，然后把类型置于尖括号内。如果是在定义该方法的类的内部，必须在点操作符之前使用 this 关键字，如果是使用 static 的方法，必须在点操作符之前加上类名。
>

用/显示的类型说明/语法/解决/泛型方法调用的结果作为参数/例子：

```java
public class ExplicitTypeSpecification{
    static void f(Map<Person, List<pet>> petPeople){}
    public static void main(String[] args){
        //f(New.map()); //Dose not compile
        f(New.<Person, List<Pet>>map());
    }
}
```

### 15.4.2 可变参数与泛型方法

### 15.4.3 用于 Generator 的泛型方法

### 15.4.4 一个通用的 Generator

### 15.4.5 简化元组的使用

### 15.4.6 一个 Set 实用工具

## 15.5 匿名内部类 ？

## 15.6 构建复杂模型 ？

## 15.7 擦除的神秘之处 ？

- 在泛型代码内部，无法获得任何有关泛型参数类型的信息。

> Java 泛型是使用擦除来实现的，这意味着当你使用泛型时，任何具体的类型信息都会被擦除了，你唯一知道的就是你在使用一个对象。因此 List<String> 和 List<Integer> 
>
> 在运行时事实上是相同的类型。这两种形式都被擦除成了它们的"原生"类型，即List。

### 15.7.1 C++的方式 

C++：模板被实例化时，模板代码知道其模板参数的类型。

Java：协助泛型类，给定泛型类的边界，以此告知编译器只能接受遵循这个边界的类型。比如重用 extends 关键字。

> 泛型类型参数将擦除到它的第一个边界（它可能会有多个边界），我们还提到了类型参数的擦除。编译器实际上会把类型参数替换为它的擦除。

### 15.7.2 迁移兼容性 

迁移兼容性：擦除的核心动机是它使得繁华的客户端可以用非泛化的类库来使用，反之亦然。

### 15.7.3 擦除的问题

> 擦除主要的正当理由是从非泛化代码的转变过程，以及在不破坏现有类库的情况下，将泛型融入 Java 语言。擦除使得现有的非泛型客户端代码能够在不改变的情况下继续使用，直至客户端准备好用泛型重写这些代码。
>
> 擦除的代价是显著的。泛型不能用于显示地引用运行时类型的操作之中。

```java
//关闭警告注解，Java SE5 之前的版本中不支持
//这个注解被放置在可以产生这类警告的方法之上，而不是整个类上。
@SupperessWarnings("unchecked")
```

### 15.7.4 边界处的动作 

> 泛型中的所有动作都发生在边界处——对传递进来的值金鼎额外的编译期检查，并插入对传递出去的值的转型。这有助于澄清对擦除的混淆，记住，“边界就是发生动作的地方”。

## 15.8 擦除的补偿 ？

### 15.8.1 创建类型实例

### 15.8.2 泛型数组

## 15.9 边界

## 15.10 通配符

- （复习）编译在左，运行在右。

```java
public static void Main(String[] args){
    Fruit[] fruit = new Apple[10];
    fruit[0] = new Apple(); //OK
    fruit[1] = new Jonathan(); //OK
    // Runtime type is Apple[]
    try{
        //Compiler allows you to add Fruit:
        fruit[0] = new Fruit(); // ArrayStoreException
    }catch(Expection e){
        System.out.println(e);
    }
}/* Output:
java.lang.ArrayStoreException:Fruit */
//泛型会在编译器就报错
public class NonCovariantGenerics{
    //Compile Error: incompatible types:
    List<Fruit> first = new ArrayList<Apple>();
}
```

- 有时想要在两个类型之间建立某种类型的向上转型关系，这正是通配符所允许的：

```java
public static void main(String[] args){
    //Wildcards allow covariance:
    List<? extends Fruit> = new ArrayList<Apple>();
    //Compile Error:can't add any type of object;
    //first.add(new Apple());
    //first.add(new Fruit());
    //first.add(new Object());
    first.add(null);//Legal but uninteresting
    //We know that it returns at least fruit:
    Fruit f = flist.get(0);
}
```

> List<? extends Fruit> 读作“具有任意从Fruit继承的类型的列表”。但这实际上并不意味着这个List将持有任何类型的Fruit。通配符引用的是明确的类型。因此它意味着“某种first应用没有指定的具体类型”
>
> 不知道List只有的具体类型，无法安全地向其中添加对象。向上转型，所以丢失掉想其中传递任何对象的能力。
>
> 调用一个返回Fruit的方法是安全的，编译器允许这么做

### 15.10.1 编译器有多聪明

### 15.10.2 逆变

- 超级类型通配符

> 可以生命通配符是由某个特定的任何基类来界定。
>
> 方法指定<? super MyClass>，甚至使用类型参数<? super T>。
>
> 不能对泛型参数给出一个超类型边界，即不能声明<T super MyClass>。

```java
public class SuperTypeWildcards {
    static void writeTo(List<? super Apple> apples){
        apples.add(new Apple());
        apples.add(new Jonathan());
        //apples.add(new Fruit()); //Error
    }
}
```

super 确定 apple 是下界，可以向其中添加 Apple 及其子类。

根据向一个泛型类型“写入”（传递给一个方法），以及如何能够从一个泛型类型中“读取”（从一个方法中返回），思考子类型和超类型的边界。超类型边界放松了在可以想方法传递的参数上所做的限制：

```java
public class GenricWriting{
    static <T> void writeExact(List<T> list, T item){
        list.add(item);
    }
    static List<Apple> apples = new ArrayList<Apple>();
    static List<Fruit> fruit = new ArrayList<Fruit>();
    static void f1(){
        writeExact(apples, new apple);
        // writeExact(fruit, new apple); //Error:
        // Incompatible tyoes: fount Fruit, required Apple
    }
    static <T> void writeWithWildcard(List<? super T> list, T item){
        list.add(item);
    }
    static void f2(){
        writeExact(apples, new apple);
        writeExact(fruit, new apple); //Error:
    }
}
```

### 15.10.3 无界通配符

> 由于泛型参数将擦除到他的第一个边界，因此 List<?> 看起来等价于 List<Object>，而 List 实际上也是 List<Object>。
>
> List 实际上表示“持有任何 Object 类型的原生 List”，而 List<?> 表示“具有某种特定类型的非原生 List，只是我们不知道那种类型是什么”

### 15.10.4 捕获转换
























































































