# 数据结构与算法分析_Java语言描述

# 第一章 引论

## 1.1 本书讨论内容

## 1.2 数学知识复习

### 1.2.1 指数

### 1.2.2 对数

### 1.2.3 级数

### 1.2.4 模运算

### 1.2.5 证明的方法

## 1.3 递归简论

- Java 提供的仅仅是遵循递归思想的一种尝试，不是所有的数学递归函数都能被有效地（或正确地）由 Java 的递归模拟来实现。
- Java 的递归方法若无基准情况是毫无意义的。
- 导致递归的四个个基本法则：
  1. 基准情形。（必须有某些基准情形，它们不用递归就能求解）
  2. 不断推进。（对于需要递归求解的情形，递归调用必须总能朝着一个基准情形推进）
  3. 设计法则。（假设所有的递归调用都能运行）
  4. 合成效益法则。（在求解一个问题的同一实例时，子恶恶在不同的递归调用中做重复性的工作）

## 1.4 实现泛型特性构件 pre-Java 5

- 面向对象的一个重要目标：对代码重用的支持。
- 支持该目标的一个重要机制：泛型。
- 1.5 版本以前，Java 并不直接支持泛型实现，泛型编程的实现是通过使用继承的一些基本概念来完成的。

### 1.4.1 使用 Object 表示泛型

两个细节考虑：

1. 为了访问这种对象的一个特定方法，必须要强制转换成正确的类型。
2. 不能使用基本类型。只有引用类型能与 Object 相容。

### 1.4.2 基本类型的包装

### 1.4.3 使用接口类型表示泛型

### 1.4.4 数组类型的兼容性

- 数组的协变性（covariant）：如果类Base是类Sub的基类，那么Base[]就是Sub[]的基类。
- 数组是协变的，而泛型是不可变（invariant）的。

数组的协变性可能会导致错误：

```java
public static  void main(String[] args) {    
        Object[] array = new String[10];
        array[0] = 10;
}
```

因为数组的协变性，他是可以通过编译的，Object[]类型的引用可以指向一个String[]类型的对象，但是运行时会报错，抛出如下异常：

```java
Exception in thread "main" java.lang.ArrayStoreException: java.lang.Integer
```

但是对于泛型就不会出现这种情况了：

```java
public static void main(String[] args) { 
        List< Object> list = new ArrayList< String>(); 
        list.add(10);
}
```

这段代码连便宜都不能通过。

> 数组是具体化的（reified），而泛型再运行时是被擦除的（erasure）。
>
> 数组是在运行时才去判断数组元素的类型约束，而泛型正好相反，在运行时，泛型的类型信息是会被擦除的，只有便宜的时候才会对类型进行强化。
>
> Java 泛型是编译器泛型，是一种语法糖，生成的二进制代码中是没有泛型的，jvm 感受不到泛型。Java 的泛型编译生成二进制代码的时候，进行了类型的擦除，放入集合的实际上是 Object 类型，从集合中获取对象的时候，获取的是 Object 类型，然后进行了强制类型转化，转换成实际的类型。

## 1.5 利用 Java 5 泛型实现泛型特性成分

### 1.5.1 简单的泛型类和接口

### 1.5.2 自动装箱/拆箱

自动装箱：一个 int 型变量被传递到需要一个 Integer 对象的地方，编译器将在幕后插入一个对 Integer 构造方法的调用。

自动拆箱：一个 Integer 对象被放到需要 int 类型变量的地方，编译器再幕后插入一个对 intValue 方法的调用。

### 1.5.3 带有限制的通配符

```java
public static double totalArea(Collection<? extends Shape> arr){
    double total = 0;
    for(Shape s : arr)
        if(s != null)
            total += s.srea();
    return total;
}
```

### 1.5.4 泛型 static 方法 ？

有时候特定类型很重要：

1. 该特定类型用作返回类型；
2. 该类型用在多于一个的参数类型中；
3. 该类型用于声明一个局部变量；

如果是这样，那么，必须要声明一种带有若干类型参数的显示泛型方法。

### 1.5.5 类型限界

```java
//在一个数组中找出最大元的泛型 static 方法
//public static <AnyType>
//public static <AnyType extends Comparable>
//public static <AnyType extends Comparable<AnyType>>
public static <AnyType extends Comparable<? super AnyType>> AnyType findMax(AnyType[] arr){
    int maxIndex = 0;
    for(int i = 1; i < arr.length; i++)
        if(arr[i].compareTo(arr[maxIndex]) > 0)
            maxIndex = i;
    return arr[maxIndex];
}
```

### 1.5.6 类型擦除

### 1.5.7 对于泛型的限制

- 基本类型

- instanceof 检测

- static 的语境
- 泛型类型的实例化

- 泛型数组对象

### 1.6 函数对象

函数对象：一个函数通过将其放在一个对象内部而被传递，这样的对象叫做函数对象。

# 第二章 算法分析

算法：是为了求解一个问题需要遵循的、被清楚指定的简单指令的集合。

## 2.1 数学基础

> 四个定义：
>
> 定义2.1：如果存在正常数 c 和 n 使得当 N>= n 时 T( N )<=cf( N )，则记为 T( N ) = O( f( N ) )。// 小于等于，传说中的大 O 标记发。
>
> 定义2.2：如果存在正常数 c 和 n 使得当 N>= n 时 T( N )>=cf( N )，则记为 T( N ) = Ω( f( N ) )。// 大于等于
>
> 定义2.3：T( N ) = θ( h( N ) ) 当且仅当  T( N ) = O( h( N ) ) 和 T( N ) = Ω( h( N ) )。// 等于
>
> 定义2.4：如果对每一常数 c 都存在常数 n 使得当 N>n 时 T( N )<cp( N )，则 T( N ) = o( p( N ) )。// 小于
>
> 比较的是相对增长率。
>

> 需要掌握的重要结论：
>
> 法则 1：
>
> 如果 T1( N ) = O( f( N ) ) 且 T2( N ) = O( g( N ) ) ，那么
>
> （a）T1( N ) + T2( N ) = O( f( N ) + g( N ) )（或非正式写成 max( O( f( N ) ) , O(g( N ) ) )）。
>
> （b）T1( N ) * T2( N ) = O( f( N ) * g( N ) )。
>
> 法则 2：
>
> 如果 $T~1( N ) $是一个 k 的多项式，则 $T( N ) = θ( N^k)$。
>
> 法则 3：
>
> 对任意常数 k，$log^kN = O(N)$。它告诉我们对数增长的非常缓慢。

## 2.2 模型

## 2.3 要分析的问题

## 2.4 运行时间计算

### 2.4.1 一个简单的例子

### 2.4.2 一般法则

- 法则 1：for 循环
- 法则 2：嵌套的 for 循环
- 法则 3：顺序语句
- 法则 4：if/else 语句

### 2.4.3 最大序列和问题的求解

算法 1：

算法 2：

算法 3：

算法 4：

```java
//最优算法 时间复杂度 O(N)
public static int maxSubSum4(int a[]){
    int maxSum = 0, thisSum = a[0];
    for(int j = 0; j < a.length; j++){
        thisSum += a[j];
        if(thisSum > maxSum)
            maxSum = thisSum;
        if(thisSum < 0) //字串和小于零，整个字串都是负收益，不要
            thisSum = 0;
    }
    return maxSum;
}
```



- 联机算法：如果数组在磁盘上或者通过互联网传送，那么他就可以被按顺序读入，在主存中不必存储数组的任何部分。不仅如此，在任意时刻，算法都能对它已经读入的数据给出子序列问题的正确答案。
- 仅需要常量空间并以线性时间运行的联机算法几乎是完美的算法。

### 2.4.4 运行时间中的对数

> 分析算法最混乱的方面大概集中在对数上。某些分治算法以 O( N log N ) 时间运行。
>
> 对数规律一般概括：如果一个算法用常熟时间（O( 1 )）将问题的大小削减为其一部分（通常是 1/2），那么该算法就是 O( log N )。另一方面，如果使用常数时间只是把问题减少一个常数的数量（如将问题减少 1 ），那么这种算法就是 O( N ) 的。

三个例子：

- 折半查找（binary search）

```java
public static <AnyType extends Comparable<? super AnyType>>
int binarySearch(AnyType a[], AnyType x){
    int low = 0, high = a.length-1;
    while (low <= high){
        int mid = (low + high)/2;
        if(a[mid].compareTo(x) > 0)
            high = mid-1;
        else if(a[mid].compareTo(x) < 0])
            low = mid+1;
        else
            return mid;
    }
    return -1;
}
```

- 欧几里得算法

```java
//假设m>=n，如果n>m，则循环第一次迭代将它们互换
public static long gcd(long m, long n){
    while(n!=0){
        long rem = m%n;
        m = n;
        n = rem;
    }
    return m;
}
```

> 算法连续计算余数直到余数是 0 为止，最后的非零余数就是最大公因数。
>
> 可以证明两次迭代以后，余数最多是原始值的一半，这就证明了迭代次数至多是 2 log N = O( log N )。证明如下：
>
> 定理：2.1 如果 M > N ，则 M mod N < M/2。
>
> 证明：存在两种情形。
>
> 1、如果 N <= M/2，余数小于 N，定理成立。
>
> 2、如果 N > M/2，余数为 M - N，也小于 M/2，定理得证。
>
> 更真实数据：
>
> 复杂度可以改进成 1.44logN，哦激励的算法迭代平均次数约为 $(12 ln2 lnN)/π^2+1.47$。

- 幂运算























































































