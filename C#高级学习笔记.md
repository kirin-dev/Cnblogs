[TOC](C#)

# 特性(Attribute)

特性(Attribute)是用于在运行时传递程序中各种元素(比如类、方法、结构、枚举、组件等)的行为信息的声明性标签。您可以通过使用特性向程序添加声明性信息。一个声明性标签是通过放置在它所应用的元素前面的方括号[]来描述的。

特性用于添加元数据，如编译器指令和注释、描述、方法、类等其他信息。.Net 框架提供了两种类型的特性：**预定义特性**和**自定义特性**。

## 规定特性

```C#
[attribute(positional_parameters, name_parameter = value, ...)]
element
```

> 特性的名称和值是在方括号内规定的，放置在它所应用的元素之前。positional_parameters规定必需的信息，name_parameter规定可选的信息。

## 预定义特性

.Net框架提供了三种预定义特性：

- AttributeUsage
- Conditional
- Obsolete

### AttributeUsage

预定义特性AttributeUsage描述了如何使用一个自定义特性类。它规定了特性可应用到的项目的类型。

```C#
[AttributeUsage(
   validon,
   AllowMultiple=allowmultiple,
   Inherited=inherited
)]
```

- 参数validon规定特性可被放置的语言元素。它是枚举器AttributeTargets的值的组合。默认值是AttributeTargets.All。
- 参数allowmultiple(可选的)为该特性的AllowMultiple属性(property)提供一个布尔值。如果为true，则该特性是多用的。默认值是false(单用的)。
- 参数inherited(可选的)为该特性的 Inherited属性(property)提供一个布尔值。如果为true，则该特性可被派生类继承。默认值是false(不被继承)。

预定义特性Conditional标记了一个条件方法，其执行依赖于指定的预处理标识符。它会引起方法调用的条件编译，取决于指定的值，比如Debug或Trace。

### Conditional

```C#
[Conditional(
   conditionalSymbol
)]
```

### Obsolete

预定义特性Obsolete标记了不应被使用的程序实体。它可以让您通知编译器丢弃某个特定的目标元素。例如，当一个新方法被用在一个类中，但是您仍然想要保持类中的旧方法，您可以通过显示一个应该使用新方法，而不是旧方法的消息，来把它标记为obsolete(过时的)。

```C#
[Obsolete(
   message
)]
[Obsolete(
   message,
   iserror
)]
```

- 参数message，是一个字符串，描述项目为什么过时以及该替代使用什么。
- 参数iserror，是一个布尔值。如果该值为true，编译器应把该项目的使用当作一个错误。默认值是false(编译器生成一个警告)。

## 自定义特性

.Net框架允许创建自定义特性，用于存储声明性的信息，且可在运行时被检索。该信息根据设计标准和应用程序需要，可与任何目标元素相关。

创建并使用自定义特性包含四个步骤：

- 声明自定义特性
- 构建自定义特性
- 在目标程序元素上应用自定义特性
- 通过反射访问特性

### 声明自定义特性

一个新的自定义特性应派生自System.Attribute类：

```C#
/* 自定义特性DeBugInfo */
[AttributeUsage(AttributeTargets.Class |
AttributeTargets.Constructor |
AttributeTargets.Field |
AttributeTargets.Method |
AttributeTargets.Property,
AllowMultiple = true)]

public class DeBugInfo : System.Attribute
```

# 反射(Reflection)

> 缺陷：性能低/模糊程序内部逻辑

- 反射允许在运行时查看特性(attribute)信息。
- 反射允许审查集合中的各种类型，以及实例化这些类型。
- 反射允许延迟绑定的方法和属性(property)。
- 反射允许在运行时创建新类型，然后使用这些类型执行一些任务。

System.Reflection类的MemberInfo对象需要被初始化，用于发现与类相关的特性(attribute)。可以定义目标类的一个对象如下：

```C#
System.Reflection.MemberInfo info = typeof(MyClass);
```

# 属性(Property)

属性是类、结构和接口的命名成员。

类或结构中的成员变量或方法称为域(Field)。

属性是域的扩展，且可使用相同的语法来访问。它们使用**访问器**(accessors)让私有域的值可被读写或操作。

属性不会确定存储位置。相反，它们具有可读写或计算它们值的访问器。

属性的访问器包含有助于获取(读取或计算)或设置(写入)属性的可执行语句。访问器声明可包含一个get访问器、一个set访问器，或者同时包含二者。

```C#
public string Name
{
   get
   {
     return name;
   }
   set
   {
     name = value;
   }
}
```

抽象类可拥有抽象属性(Abstract Properties)，这些属性应在派生类中被实现。

```C#
public abstract class Person
{
   public abstract string Name { get; set; }
   public abstract int Age { get; set; }
}
public class Student : Person
{
   public string Code { get; set; } = "N.A";
   public override string Name { get; set; } = "N.A";
   public override int Age { get; set; } = 0;
   public override string ToString()
   {
      return $"Code:{Code},Name:{Name},Age:{Age}";
   }
}
```

# 索引器(Indexer)

索引器允许一个对象可以像数组一样使用下标的方式来访问。

```C#
element-type this[int index]
{
   // get 访问器
   get
   {
      // 返回 index 指定的值
   }

   // set 访问器
   set
   {
      // 设置 index 指定的值
   }
}
```

> 索引器可被重载。索引器声明的时候也可带有多个参数，且每个参数可以是不同的类型。没有必要让索引器必须是整型的。C#允许索引器可以是其他类型，例如，字符串类型。

# 委托(Delegate)

C#中的委托(Delegate)类似于C/C++中函数的指针。

委托特别用于实现事件和回调方法。所有的委托都派生自System.Delegate类。

## 声明委托

```C#
delegate <return type> <delegate-name> <parameter list>
```

## 实例化委托

一旦声明了委托类型，委托对象必须使用new关键字来创建，且与一个特定的方法有关。当创建委托时，传递到new语句的参数就像方法调用一样书写，但是不带有参数。

```C#
public delegate void printString(string s);
...
printString ps1 = new printString(WriteToScreen);
printString ps2 = new printString(WriteToFile);
```

## 委托的多播

委托对象可使用"+"运算符进行合并。一个合并委托调用它所合并的两个委托。只有相同类型的委托可被合并。"-"运算符可用于从合并的委托中移除组件委托。

# 事件(Event)

事件基本上说是一个用户操作，如按键、点击、鼠标移动等等，或者是一些提示信息，如系统生成的通知。应用程序需要在事件发生时响应事件，例如中断。

C#中使用事件机制实现线程间的通信。

## 通过事件使用委托

事件在类中声明且生成，且通过使用同一个类或其他类中的委托与事件处理程序关联。包含事件的类用于发布事件。这被称为**发布器**(publisher)类。其他接受该事件的类被称为**订阅器**(subscriber)类。事件使用发布-订阅(publisher-subscriber)模型。

- 发布器是一个包含事件和委托定义的对象。事件和委托之间的联系也定义在这个对象中。发布器类的对象调用这个事件，并通知其他的对象。
- 订阅器是一个接受事件并提供事件处理程序的对象。在发布器类中的委托调用订阅器类中的方法(事件处理程序)。

## 声明事件

在类的内部声明事件，首先必须声明该事件的委托类型。例如：

```C#
public delegate void BoilerLogHandler(string status);
// 然后，声明事件本身，使用event关键字：
// 基于上面的委托定义事件
public event BoilerLogHandler BoilerEventLog;
```

上面的代码定义了一个名为BoilerLogHandler的委托和一个名为BoilerEventLog的事件，该事件在生成的时候会调用委托。

# 集合(Collection)

集合类是专门用于数据存储和检索的类。这些类提供了对栈、队列、列表和哈希表的支持。大多数集合类实现了相同的接口。

![img](https://img2022.cnblogs.com/blog/2975286/202211/2975286-20221122102715151-1581212731.png)

# 泛型(Generic)

泛型允许您延迟编写类或方法中的编程元素的数据类型的规范，直到实际在程序中使用它的时候。换句话说，泛型允许您编写一个可以与任何数据类型一起工作的类或方法。

使用泛型是一种增强程序功能的技术，具体表现在以下几个方面：

- 它有助于您最大限度地重用代码、保护类型的安全以及提高性能。
- 您可以创建泛型集合类。.NET框架类库在System.Collections.Generic命名空间中包含了一些新的泛型集合类。您可以使用这些泛型集合类来替代System.Collections中的集合类。
- 您可以创建自己的泛型接口、泛型类、泛型方法、泛型事件和泛型委托。
- 您可以对泛型类进行约束以访问特定数据类型的方法。
- 关于泛型数据类型中使用的类型的信息可在运行时通过使用反射获取。

还可以通过类型参数定义泛型委托：

```C#
delegate T NumberChanger<T>(T n);
```

# 匿名方法(Anonymous methods)

匿名方法提供了一种传递代码块作为委托参数的技术。匿名方法是没有名称只有主体的方法。在匿名方法中您不需要指定返回类型，它是从方法主体内的return语句推断的。

匿名方法是通过使用 delegate 关键字创建委托实例来声明的。

```C#
delegate void NumberChanger(int n);
...
NumberChanger nc = delegate(int x)
{
    Console.WriteLine("Anonymous Method: {0}", x);//anonymous method
};
```

委托可以通过匿名方法调用，也可以通过命名方法调用，即通过向委托对象传递方法参数。

注意: 匿名方法的主体后面需要一个;。

```C#
nc(10);
```

# 不安全代码

当一个代码块使用unsafe修饰符标记时，C#允许在函数中使用指针变量。不安全代码或非托管代码是指使用了指针变量的代码块。

指针变量：

```C#
type* var-name;
```

> 可以使用ToString()方法检索存储在指针变量所引用位置的数据。
> 可以向方法传递指针变量作为方法的参数。
> 可以使用指针访问数组元素。
> 可以像C/C++一样，使用fixed关键字来固定指针。

> 为了编译不安全代码，您必须切换到命令行编译器指定/unsafe命令行。eg:csc /unsafe prog.cs

# 多线程

在C#中，System.Threading.Thread类用于线程的工作。它允许创建并访问多线程应用程序中的单个线程。进程中第一个被执行的线程称为主线程。

当C#程序开始执行时，主线程自动创建。使用Thread类创建的线程被主线程的子线程调用。可以使用Thread类的CurrentThread属性访问线程。

## 创建线程

线程是通过扩展Thread类创建的。扩展的Thread类调用Start()方法来开始子线程的执行：

```C#
using System;
using System.Threading;

namespace MultithreadingApplication
{
    class ThreadCreationProgram
    {
        public static void CallToChildThread()
        {
            Console.WriteLine("Child thread starts");
        }
       
        static void Main(string[] args)
        {
            ThreadStart childref = new ThreadStart(CallToChildThread);
            Console.WriteLine("In Main: Creating the Child thread");
            Thread childThread = new Thread(childref);
            childThread.Start();
            Console.ReadKey();
        }
    }
}
```

## 管理线程

eg:sleep()用于在一个特定的时间暂停线程。

## 销毁线程

Abort()方法用于销毁线程。

通过抛出threadabortexception在运行时中止线程。这个异常不能被捕获，如果有finally块，控制会被送至finally块。
