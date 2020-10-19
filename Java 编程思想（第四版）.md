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
























































































