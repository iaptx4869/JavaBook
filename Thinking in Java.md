# 第1章 对象导论

## 1.1 抽象过程

1. 万物皆为对象。

2. 程序是对象的集合，它们通过发送消息来告知彼此所要做的。

3. 每个对象都有自己的由其他对象所构成的存储。

4. 每个对象都拥有其类型。

5. 某一特定类型的所有对象都可以接收同样的消息。

# 第2章 一切都是对象

## 2.1 用引用操纵对象

操纵的标识符实际上是对象的一个“引用”（reference）。

创建一个String引用：

```java
String s;
```

创建一个引用的同时便进行初始化：

```java
String s = "asdf";
```

字符串可以用带引号的文本初始化。通常，必须对对象采用一种更通用的初始化方法。

## 2.2 必须由你创建所有对象

```java
String s = new String("asdf");
```

### 2.2.1 存储到什么地方

1. 寄存器。
2. 堆栈。
3. 堆。
4. 常量存储。
5. 非RAM存储。

### 2.2.2 特例：基本类型

不用new来创建变量，而是创建一个并非是引用的“自动”变量。这个变量直接存储“值”，并置于堆栈中，因此更加高效。

| 基本类型    | 大小      | 最小值       | 最大值             | 包装器类型     |
| ------- | ------- | --------- | --------------- | --------- |
| boolean | -       | -         | -               | Boolean   |
| char    | 16 bits | Unicode 0 | Unicode 2^16^-1 | Character |
| byte    | 8 bits  | -128      | +127            | Byte      |
| short   | 16 bits | -2^15^    | +2^15^-1        | Short     |
| int     | 32 bits | -2^31^    | +2^31^-1        | Integer   |
| long    | 64 bits | -2^63^    | +2^63^-1        | Long      |
| float   | 32 bits | IEEE754   | IEEE754         | Float     |
| double  | 64 bits | IEEE754   | IEEE754         | Double    |
| void    | -       | -         | -               | Void      |

boolean类型所占存储空间的大小没有明确指定，仅定义为能够取字面值true或false

基本类型具有的包装器类，使得可以在堆中创建一个非基本对象，用来表示对应的基本类型。例如：

```java
char c = 'x';
Character ch = new Character(c);
```

也可以这样用：

```java
Character ch = new Character('x');
```

Java SE5的自动包装功能将自动地将基本类型转换为包装器类型：

```java
Character ch = 'x';
```

并可以反向转换：

```java
char c = ch;
```

**高精度数字**

Java提供了两个用于高精度计算的类：BigInteger和BigDecimal。虽然它们大体上属于“包装器类”的范畴，但二者都没有对应的基本类型。

能作用于int或float的操作，也同样能作用于BigInteger或BigDecimal。只不过必须以方法调用方式取代运算符方式来实现。以速度换取了精度。

## 2.3 永远不需要销毁对象

### 2.3.1 作用域

作用域由花括号的位置决定。

```java
{
  int x = 12;
  // Only x available
  {
    int q = 96;
    // Both x & q available
  }
  // Only x available
  // q is "out of scope" 
}
```

### 2.3.2 对象的作用域

```java
{
  String s = new String("a string");
} // End of scope
```

引用s在作用域终点就消失了。然而，s指向的String对象仍继续占据内存空间。

## 2.4 创建新的数据类型：类

class这个关键字之后紧跟着的是新类型的名称。例如：

```java
class ATypeName { /* Class body gose here */ }
```

这就引入了一种新的类型，尽管类主体仅包含一条注释语句。

已经可以用new来创建这种类型的对象：

```java
ATypeName a = new ATypeName();
```

### 2.4.1 字段和方法

一旦定义了一个类（在Java中所作的全部工作就是定义类，产生那些类的对象，以及发送消息给这些对象），就可以在类中设置两种类型的元素：字段（有时被称作数据成员）和方法（有时被称作成员函数）。字段可以是任何类型的对象，可以通过其引用与其进行通信；也可以是基本类型中的一种。如果字段是对某个对象的引用，那么必须初始化该引用，以便使其与一个实际的对象相关联。

每个对象都有用来存储其字段的空间；普通字段不能再对象间共享。下面是一个具有某些字段的类：

```java
class DataOnly {
  int i;
  double d;
  boolean b;
}
```

尽管这个类除了存储数据之外什么也不能做，但是仍旧可以像下面这样创建它的一个对象：

```java
DataOnly data = new DataOnly();
```

可以给字段赋值，但首先必须知道如何引用一个对象的成员。具体的实现为：在对象引用的名称之后紧接着一个句点，然后再接着是对象内部的成员名称：

```java
objectReference.memeber
```

例如：

```java
data.i = 47;
data.d = 1.1;
data.b = false;
```

想修改的数据也有可能位于对象所包含的其他对象中。在这种情况下，只需要再使用连接句点即可。例如：

```java
myPlane.leftTank.capacity = 100;
```

DataOnly类除了保存数据外没别的用处，因为它没有任何成员方法。

**基本成员默认值**

若类的某个成员是基本数据类型，即使没有进行初始化，Java也会确保它获得一个默认值：

| 基本类型    | 默认值            |
| ------- | -------------- |
| boolean | false          |
| char    | '\u0000'(null) |
| byte    | (byte)0        |
| short   | (short)0       |
| int     | 0              |
| long    | 0L             |
| float   | 0.0f           |
| double  | 0.0d           |

当变量作为类的成员使用时，Java才确保给定其默认值，以确保那些是基本类型的成员变量得到初始化，防止产生程序错误。但是，这些初始值对程序来说，可能是不正确的，甚至是不合法的。所以最好明确地对变量进行初始化。

然而上述确保初始化的方法并不适用于“局部”变量（即并非某个类的字段）。因此，如果在某个方法定义中有

```java
int x;
```

那么变量x得到的可能是任意值，而不会被自动初始化为零。所以在使用x前。应先对其赋一个适当的值。如果忘记了这么做，Java会在编译时返回一个错误，告诉你此变量没有初始化。

## 2.5 方法、参数和返回值

Java的方法决定了一个对象能够接收什么样的信息。方法的组成部分包括：名称、参数、返回值和方法体。下面是它最基本的形式：

```java
ReturnType methodName（ /* Argument list */ ) {
  /* Method body */
}
```

返回类型描述的是在调用方法之后从方法返回的值。参数列表给出了要传给方法的信息的类型和名称。方法名和参数列表（它们合起来被称为“方法签名”）唯一地标识出某个方法。

调用方法的行为通常被称为发送消息给对象。面向对象的程序设计通常简单地归纳为“向对象发送消息”。

### 2.5.1 参数列表

方法的参数列表指定要传递给方法什么样的信息。

## 2.6 构建一个Java程序

### 2.6.1 名字可见性

所有文件都能够自动存活于它们自己的名字空间内，而且同一个文件内的每个类都有唯一的标识符。

### 2.6.2 运用其他构件

```java
import java.util.ArrayList;
```

```java
import java.util.*;
```

### 2.6.3 static关键字

通常来说，当创建类时，就是在描述那个类的对象的外观与行为。除非用new创建那个类的对象，否则，实际上并未获得任何对象。执行new来创建对象时，数据存储空间才被分配，其方法才供外界调用。

有两种情形用上述方法是无法解决的。一种情形是，只想为某特定域分配单一储存空间，而不去考虑究竟要创建多少对象，甚至根本就不创建任何对象。另一种情形是，希望某个方法不与包含它的类的任何对象关联在一起。也就是说，即使没有创建对象，也能够调用这个方法。

通过static关键字可以满足这两方面的需要。当声明一个事物是static时，就意味着这个域或方法不会与包含它的那个类的任何对象实例关联在一起。所以，即使从未创建某个类的任何对象，也可以调用其static方法或访问其static域。通常，你必须创建一个对象，并用它来访问数据或方法。因为非static域和方法必须知道它们一起运作的特定对象[^2.6.3.1]。

[^2.6.3.1]: 当然，由于在用static方法前不需要创建任何对象；所以对于static方法，不能简单地通过调用其他非static域或方法而没有指定某个命名对象，来直接访问非static域或方法（因为非static域或方法必须与某一特定对象关联）。

有些面向对象语言采用*类数据*和*类方法*两个术语，代表那些数据和方法只是作为整个类，而不是类的摸个特定对象而存在的。

只须将static关键字放在定义之前，就可以将字段或方法设定为static。例如，下面的代码就生成了一个static字段，并对其进行了初始化：

```java
class StaticTest{
  static int i = 47;
}
```

现在，即使你创建了两个StaticTest对象，StaticTest.i也只有一份存储空间，这两个对象共享一个i。

```java
StaticTest st1 = new StaticTest();
StaticTest st2 = new StaticTest();
```

在这里，st1.i和st2.i指向同一存储空间，因此它们具有相同的值47。

引用static变量有两种方法。如前例所示，可以通过一个对象去定位它，如st2.i；也可以通过其类名直接引用，而这对于非静态成员则不行。

```java
StaticTest.i++
```

此时，st1.i和st2.i仍具有相同的值48。

使用类名是引用static变量的首选方式，这不仅是因为它强调了变量的static结构，而且在某些情况下它还为编译器进行优化提供了更好的机会。

类似所及也应用于静态方法。既可以像其他方法一样，通过一个对象来引用某个静态方法，也可以通过特殊的语法形式ClassName.method()加以引用。定义静态方法的方式也与定义静态变量的方式相似：

```java
class Incrementable {
  static void increment(){ StaticTest.i++; }
}
```

可以看到，Incrementable的increment()方法通过++运算符将静态数据i递加。可以采用典型的方式，通过对象来调用increment()：

```java
Incrementable sf = new Incrementable();
sf.increment();
```

或者，因为increment()是一个静态方法，所以也可以通过它的类直接调用：

```java
Incrementable.increment();
```

尽管当static作用于某个字段时，肯定会改变数据创建的方式（因为一个static字段对每个类来说都只有一份存储空间，而非static字段则是对每个对象有一个存储空间），但是如果static作用于某个方法，差距却没有那么大。static方法的一个重要用法就是在不创建任何对象的前提下就可以调用它。正如我们将会看到的那样，这一点对于定义main()方法很重要，这个方法是运行一个应用时的入口点。

和其他任何方法一样，static方法可以创建或使用与其类型相同的被命名对象，因此，static方法常常拿来做“牧羊人”的角色，负责看护与其隶属于同一类型的实例群。

## 2.7 你的第一个Java程序

```java
// HelloDate.java
import java.util.*;

public class HelloDate {
  public static void main(String[] args){
    System.out.println("Hello, it's: ");
    System.out.println(new Date());
  }
}
```

有一个特定类会自动被导入到每一个Java文件中:Java.long。

main()方法的参数是一个String对象的数组，在这个程序中并未用到args，但是Java编译器要求必须这样做，因为args要用来存储命令行参数。

```java
public class ShowProperties {
    public static void main(String[] args) {
        System.getProperties().list(System.out);
        System.out.println(System.getProperty("user.name"));
        System.out.println(System.getProperty("java.library.path"));
    }
}
```

main()的第一行将显示从运行程序的系统中获取的所有“属性”，因此它可以向你提供环境信息。list()方法将结果发送给它的参数：System.out。

### 2.7.1 编译和运行

```
javac HelloDate.java
```

```
java HelloDate
```

## 2.8 注释和嵌入式文档

### 2.8.1 注释文档

javadoc便是用于提取注释的工具，它是JDK安装的一部分。它采用了Java编译器的某些技术，查找程序内的特殊注释标签。它不仅解析由这些标签标记的信息，也将毗邻注释的类名或方法名抽取出来。如此，我们就可以用最少的工作量，生成相当好的程序文档。

### 2.8.2 语法

所有javadoc命令都只能在“/\*\*”注释中出现，和通常一样，注释结束于“\*/”

javadoc只能为public（公共）和protected（受保护）成员进行文档注释。pirvate（私有）和包内可访问成员的注释会被忽略掉，所以输出结果中看不到它们（不过可以用-private进行标记，以便把private成员的注释也包括在内）。

### 2.8.3 嵌入式HTML

```java
/**
* You can <em>even<em> insert a list:
* <ol>
* <li> Item one
* <li> Item two
* <li> Item three
* </ol>
*/
```

在文档注释中，位于每一行开头的星号和前导空格都会被javadoc丢弃。javadoc会对所有内容重新格式化，使其与标准的文档外观一致。不要再嵌入式HTML中使用标题标签，例如\<h1>或\<hr>，因为javadoc会插入自己的标题，而你的标题可能同它们发生冲突。

所有类型的注释文档——类、域和方法——都支持嵌入式HTML。

## 2.9 编码风格

“驼峰风格”

