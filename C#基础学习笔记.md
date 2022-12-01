[TOC](C#)

- 类似C/C++，面向对象
- 类似Java，功能强大，有很多好用的特性

# Hello, World

hello.cs:

```C#
using System;
namespace HelloWorldApplication //命名空间声明
{
    class HelloWorld //类
    {
        static void Main(string[] args) //主函数
        {
            /* Print */ //注释
            Console.WriteLine("Hello, World"); //打印
            Console.ReadLine(); //等待输入提示程序结束
        }
    }
}
```

> 与Java不同的是，C#的文件名可以不同于类的名称。

# 基本语法

## 经典实例

```C#
using System;
namespace RectangleApplication
{
    class Rectangle
    {
        /* 成员变量 */
        double length;
        double width;
        /* 成员函数 */
        public void Acceptdetails()
        {
            length = 4.5;
            width = 3.5;
        }
        public double GetArea()
        {
            return length * width;
        }
        public void Display()
        {
            Console.WriteLine("Length: {0}", length);
            Console.WriteLine("Width: {0}", width);
            Console.WriteLine("Area: {0}", GetArea());
        }
    }

    class ExecuteRectangle
    {
        static void Main(string[] args)
        {
            /* 主函数 */
            Rectangle r = new Rectangle(); //实例化
            r.Acceptdetails();
            r.Display();
            Console.ReadLine();
        }
    }
}
```

## 标识符

**标识符**是用来识别类、变量、函数或任何其它用户定义的项目。在 C# 中，类的命名必须遵循如下基本规则：

- 标识符必须以字母、下划线或@开头，后面可以跟一系列的字母、数字( 0 - 9 )、下划线( _ )、@。
- 标识符必须不包含任何嵌入的空格或符号，比如 ? - + ! # % ^ & * ( ) [] { } . ; : " ' / \。
- 标识符不能是 C# 关键字。除非它们有一个@前缀。 例如，@if是有效的标识符，但if不是，因为if是关键字。
- 不能与C#的类库名称相同。

## C#关键字

![image](https://img2022.cnblogs.com/blog/2975286/202211/2975286-20221121112409526-675680714.jpg)

# 数据类型

## 值类型(Value types)

与C/C++中的数据类型完全相同。

> 可以用sizeof(value type)获取大小，这也和C/C++中一样。

## 引用类型(Reference types)

### 对象(Object)类型

对象类型是C#通用类型系统(Common Type System - CTS)中所有数据类型的终极基类。Object是System.Object类的别名。所以对象类型可以被分配任何其他类型(值类型、引用类型、预定义类型或用户自定义类型)的值。但是，在分配值之前，需要先进行类型转换。

当一个值类型转换为对象类型时，则被称为**装箱**；另一方面，当一个对象类型转换为值类型时，则被称为**拆箱**。

```C#
Object obj;
obj = 100; //装箱
```

### 动态(Dynamic)类型

您可以存储任何类型的值在动态数据类型变量中。这些变量的类型检查是在运行时发生的。

```C#
dynamic <variable_name> = value;
```

> 动态类型与对象类型相似，但是对象类型变量的类型检查是在编译时发生的，而动态类型变量的类型检查是在运行时发生的。

### 字符串(String)类型

字符串类型允许您给变量分配任何字符串值。字符串类型是System.String类的别名。它是从对象类型派生的。字符串类型的值可以通过两种形式进行分配：引号和@引号。

```C#
String str = "kirin-dev";
@"kirin-dev";
```

> C#string字符串的前面可以加@(称作"逐字字符串")将转义字符(\)当作普通字符对待

> @字符串中可以任意换行，换行符及缩进空格都计算在字符串长度之内

### 指针类型(Pointer types)

指针类型变量存储另一种类型的内存地址。C#中的指针与 C/C++中的指针有相同的功能。

```C#
type* identifier;
```

# 类型转换

- 隐式类型转换：C#默认的以安全方式进行的转换，不会导致数据丢失。例如，从小的整数类型转换为大的整数类型，从派生类转换为基类。
- 显式类型转换：即强制类型转换。显式转换需要强制转换运算符，而且强制转换会造成数据丢失。

![image](https://img2022.cnblogs.com/blog/2975286/202211/2975286-20221121115122943-1268289809.jpg)

# 变量、常量、运算符、判断、循环

C#变量声明、常量声明、运算符、判断语句、循环语句、与C/C++完全相同。

# 封装

与C/C++面向对象类似。

C#封装根据具体的需要，设置使用者的访问权限，并通过**访问修饰符**来实现。访问修饰符定义了一个类成员的范围和可见性。

C#支持的访问修饰符如下所示：

- public：所有对象都可以访问；
- private：对象本身在对象内部可以访问；
- protected：只有该类对象及其子类对象可以访问
- internal：同一个程序集的对象可以访问；
- protected internal：访问限于当前程序集或派生自包含类的类型。

![img](https://img2022.cnblogs.com/blog/2975286/202211/2975286-20221121141328483-851425062.png)

# 方法

## 定义方法

```C#
<Access Specifier> <Return Type> <Method Name>(Parameter List)
{
   Method Body
}
```

- Access Specifier：访问修饰符，这个决定了变量或方法对于另一个类的可见性。
- Return type：返回类型，一个方法可以返回一个值。返回类型是方法返回的值的数据类型。如果方法不返回任何值，则返回类型为 void。
- Method name：方法名称，是一个唯一的标识符，且是大小写敏感的。它不能与类中声明的其他标识符相同。
- Parameter list：参数列表，使用圆括号括起来，该参数是用来传递和接收方法的数据。参数列表是指方法的参数类型、顺序和数量。参数是可选的，也就是说，一个方法可能不包含参数。
- Method body：方法主体，包含了完成任务所需的指令集。

## 调用方法

一般调用/递归调用

## 参数传递

- 按值传参
- 按引用传参eg:public void swap(ref int x, ref int y)
- 按输出传参eg:public void getValues(out int x, out int y )

# 可空类型(Nullable)

?用于对int、double、bool等无法直接赋值为null的数据类型进行null的赋值，意思是这个数据类型是Nullable类型的。

??用于判断一个变量在为null的时候返回一个指定的值。

```C#
<data_type>? <variable_name> = null;
```

```C#
int? i = 3;
/* 等价 */
Nullable<int> i = new Nullable<int>(3);
```

```C#
num0 = num1 ?? 5.34; //num1如果为空值则返回5.34，否则返回num1
```

# 数组

```C#
datatype[] arrayName;
```

- datatype用于指定被存储在数组中的元素的类型。
- []指定数组的秩(维度)。秩指定数组的大小。
- arrayName指定数组的名称。

初始化：

```C#
double[] balance = new double[10];
int[] marks = new int[5]{99, 98, 92, 97, 95};
```

# 字符串

```C#
using System;

namespace StringApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            /* 字符串，字符串连接 */
            string fname, lname;
            fname = "Rowan";
            lname = "Atkinson";

            string fullname = fname + lname;
            Console.WriteLine("Full Name: {0}", fullname);
            /* Full Name: RowanAtkinson */

            /* 通过使用 string 构造函数 */
            char[] letters = { 'H', 'e', 'l', 'l','o' };
            string greetings = new string(letters);
            Console.WriteLine("Greetings: {0}", greetings);
            /* Greetings: Hello */

            /* 方法返回字符串 */
            string[] sarray = { "Hello", "From", "Tutorials", "Point" };
            string message = String.Join(" ", sarray);
            Console.WriteLine("Message: {0}", message);
            /* Message: Hello From Tutorials Point */

            /* 用于转化值的格式化方法 */
            DateTime waiting = new DateTime(2012, 10, 10, 17, 58, 1);
            string chat = String.Format("Message sent at {0:t} on {0:D}",
            waiting);
            Console.WriteLine("Message: {0}", chat);
            /* Message: Message sent at 17:58 on Wednesday, 10 October 2012 */
            Console.ReadKey() ;
        }
    }
}
```

String类有以下两个属性：

- Chars:在当前 String 对象中获取 Char 对象的指定位置。
- Length:在当前的 String 对象中获取字符数。

类似C/C++，C#的String类有许多方法用于string对象的操作。

# 结构体

在C#中的结构与传统的C或C++中的结构不同。C#中的结构有以下特点：

- 结构可带有方法、字段、索引、属性、运算符方法和事件。
- 结构可定义构造函数，但不能定义析构函数。但是，您不能为结构定义无参构造函数。无参构造函数(默认)是自动定义的，且不能被改变。
- 与类不同，结构不能继承其他的结构或类。
- 结构不能作为其他结构或类的基础结构。
- 结构可实现一个或多个接口。
- 结构成员不能指定为 abstract、virtual 或 protected。
- 当您使用 New 操作符创建一个结构对象时，会调用适当的构造函数来创建结构。与类不同，结构可以不使用 New 操作符即可被实例化。
- 如果不使用 New 操作符，只有在所有的字段都被初始化之后，字段才被赋值，对象才被使用。

类和结构有以下几个基本的不同点：

- 类是引用类型，结构是值类型。
- 结构不支持继承。
- 结构不能声明默认的构造函数。

# 枚举

枚举是一组命名整型常量。枚举类型是使用enum关键字声明的。

C#枚举是值类型。换句话说，枚举包含自己的值，且不能继承或传递继承。

```C#
enum <enum_name>
{ 
    enumeration list 
};
```

- enum_name 指定枚举的类型名称。
- enumeration list 是一个用逗号分隔的标识符列表。

# 类

```C#
<access specifier> class  class_name
{
    // member variables
    <access specifier> <data type> variable1;
    <access specifier> <data type> variable2;
    ...
    <access specifier> <data type> variableN;
    // member methods
    <access specifier> <return type> method1(parameter_list)
    {
        // method body
    }
    <access specifier> <return type> method2(parameter_list)
    {
        // method body
    }
    ...
    <access specifier> <return type> methodN(parameter_list)
    {
        // method body
    }
}
```

- 访问标识符access specifier指定了对类及其成员的访问规则。如果没有指定，则使用默认的访问标识符。类的默认访问标识符是 internal，成员的默认访问标识符是 private。
- 数据类型data type指定了变量的类型，返回类型return type指定了返回的方法返回的数据类型。
- 如果要访问类的成员，你要使用点(.)运算符。
- 点运算符链接了对象的名称和成员的名称。

## 成员函数

类的成员函数是一个在类定义中有它的定义或原型的函数，就像其他变量一样。作为类的一个成员，它能在类的任何对象上操作，且能访问该对象的类的所有成员。

成员变量是对象的属性(从设计角度)，且它们保持私有来实现封装。这些变量只能使用公共成员函数来访问。

## 构造函数

类的构造函数是类的一个特殊的成员函数，当创建类的新对象时执行。

构造函数的名称与类的名称完全相同，它没有任何返回类型。

## 析构函数

类的析构函数是类的一个特殊的成员函数，当类的对象超出范围时执行。

析构函数的名称是在类的名称前加上一个波浪形(~)作为前缀，它不返回值，也不带任何参数。

析构函数用于在结束程序（比如关闭文件、释放内存等）之前释放资源。析构函数不能继承或重载。

## 静态成员

可以使用static关键字把类成员定义为静态的。当我们声明一个类成员为静态时，意味着无论有多少个类的对象被创建，只会有一个该静态成员的副本。

关键字static意味着类中只有一个该成员的实例。静态变量用于定义常量，因为它们的值可以通过直接调用类而不需要创建类的实例来获取。静态变量可在成员函数或类的定义外部进行初始化。也可以在类的定义内部初始化静态变量。

# 继承

继承允许根据一个类来定义另一个类。

当创建一个类时，程序员不需要完全重新编写新的数据成员和成员函数，只需要设计一个新的类，继承了已有的类的成员即可。这个已有的类被称为的基类，这个新的类被称为派生类。

```C#
<访问修饰符> class <基类>
{
 ...
}
class <派生类> : <基类>
{
 ...
}
```

# 多态

## 静态多态性

- 函数重载
- 运算符重载

## 动态多态性

- 抽象类abstract
- 虚方法virtual

# 接口

接口定义了所有类继承接口时应遵循的语法合同。接口定义了语法合同 "是什么" 部分，派生类定义了语法合同 "怎么做" 部分。

接口定义了属性、方法和事件，这些都是接口的成员。接口只包含了成员的声明。成员的定义是派生类的责任。接口提供了派生类应遵循的标准结构。

接口使得实现接口的类或结构在形式上保持一致。

抽象类在某种程度上与接口类似，但是，它们大多只是用在当只有少数方法由基类声明由派生类实现时。

接口本身并不实现任何功能，它只是和声明实现该接口的对象订立一个必须实现哪些行为的契约。

抽象类不能直接实例化，但允许派生出具体的，具有实际功能的类。

接口使用interface关键字声明，它与类的声明类似。接口声明默认是public的。

```C#
interface IMyInterface
{
    void MethodToImplement();
}
```

以上代码定义了接口IMyInterface。通常接口命令以I字母开头，这个接口只有一个方法MethodToImplement()，没有参数和返回值，当然我们可以按照需求设置参数和返回值。

# 命名空间

```C#
namespace namespace_name
{
   // 代码声明
}
```

```C#
namespace_name.item_name;
```

using 关键字表明程序使用的是给定命名空间中的名称。例如，我们在程序中使用System命名空间，其中定义了类Console。我们可以只写：

```C#
Console.WriteLine ("Hello there");
```

等价于

```C#
System.Console.WriteLine("Hello there");
```

也可以使用using命名空间指令，这样在使用的时候就不用在前面加上命名空间名称。该指令告诉编译器随后的代码使用了指定命名空间中的名称。

命名空间可以被嵌套。

可以使用点(.)运算符访问嵌套的命名空间的成员。

# 预处理器指令

C#编译器没有一个单独的预处理器，但是，指令被处理时就像是有一个单独的预处理器一样。在C#中，预处理器指令用于在条件编译中起作用。与C/C++不同的是，它们不是用来创建宏。一个预处理器指令必须是该行上的唯一指令。

![img](https://img2022.cnblogs.com/blog/2975286/202211/2975286-20221121173721821-451400851.png)

# 正则表达式

正则表达式是一种匹配输入文本的模式。

.Net 框架提供了允许这种匹配的正则表达式引擎。

模式由一个或多个字符、运算符和结构组成。

# 异常处理

C#异常处理时建立在四个关键词之上的：try、catch、finally和throw。

- try：一个try块标识了一个将被激活的特定的异常的代码块。后跟一个或多个catch块。
- catch：程序通过异常处理程序捕获异常。catch关键字表示异常的捕获。
- finally：finally块用于执行给定的语句，不管异常是否被抛出都会执行。例如，如果您打开一个文件，不管是否出现异常文件都要被关闭。
- throw：当问题出现时，程序抛出一个异常。使用throw关键字来完成。

# 文件的输入与输出

- I/O类
- FileStream类
