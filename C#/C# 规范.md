# C# 程序结构

1. 命名空间（Namespace）

- 命名空间用于组织代码，防止命名冲突，并提供代码的逻辑分组 
- 使用 `using` 关键字导入命名空间 

2. 类（Class）

- 类是C#中的基本代码单元，用于定义对象的属性和方法
- 类可以包含构造函数、属性、方法、字段等
- 可以继承一个基类，支持单继承

3. 方法（Method）

- 方法是类中的函数，用于执行特定的操作
- 方法包括方法名、参数列表、返回类型和方法体

4. 属性（Property）

- 属性提供对私有字段的访问，可以控制对字段的读写操作
- 属性包括 get 和 set 访问器，用于获取和设置属性的值

5. 字段（Field）

- 字段是类中用于存储数据的变量。 
- 字段可以用不同的访问修饰符来控制访问权限。 

6. 接口（Interface）

- 接口是一种契约，定义了类应该实现的成员。 
- 类可以实现多个接口，实现接口中定义的成员。 

7. 结构（Struct）

- 结构是一种轻量级的类，适合用于小型数据类型。 
- 结构可以有构造函数、属性、方法等。 

8. 枚举（Enum）

- 枚举是一种特殊的值类型，用于定义一组命名的常量
- 枚举常用于表示有限的一组选项

9. 委托（Delegate）

- 委托是一种用于引用方法的类型，通常用于事件处理和回调机制

10. 事件（Event）

- 事件允许类通知其他类发生了特定的动作，用于实现观察者模式

11. 数组（Array）

- 数组是一种存储多个相同类型元素的集合
- 数组长度固定，通过索引访问元素



```c#
using System;

// 命名空间 BasicCSharpProgram：命名空间用于组织代码，避免命名冲突
namespace BasicCSharpProgram
{
    // 类 Program，是C#中的基本代码单元，可以包含字段、属性、方法等
    internal class Program
    {
        // 属性：包含 get 和 set 访问器，用于访问字段 "Hello, "
        public string GreetingMessage { get; set; } = "Hello, ";

        // 方法：接受一个参数 name，将问候信息和 name 组合后输出到控制台
        public void DisplayGreeting(string name)
        {
            var message = GreetingMessage + name;
            Console.WriteLine(message);
        }

        // Main 方法: 是程序的入口点
        private static void Main(string[] args)
        {
            // 创建 Program 类的实例
            var program = new Program();

            // 调用 DisplayGreeting 方法
            program.DisplayGreeting("Alice");

            // 设置属性值
            program.GreetingMessage = "Hi, ";
            program.DisplayGreeting("Bob");
        }
    }
}
```



# 变量命名规则

> 变量命名规则

 1. 变量名称可以包含所有字母数字字符（[A-Z]、[a-z]、[0-9]）、“_”（下划线）

 2. 变量名不能以数字开头

 3. 变量名称不能是任何 C# 关键字，例如 int、float、null、string 等

 4. 不能在同一个作用域（例如同一个命名空间、类、结构体、或接口）中定义两个具有相同名称的变量。这将引发命名冲突并导致编译错误

    

> 声明变量必须遵循的规则

 1. 指定其类型（例如 int）
 2. 指定其名字（例如 name）
 3. 可以提供初始值（如 22）



> 初始化变量

 术语初始化意味着为变量分配一些值。基本上，变量的实际使用属于初始化部分
 C# 中，每种数据类型都有一些默认值，当给定变量没有显式设置值时使用该默认值
 初始化可以单独完成，也可以与声明一起完成



> 初始化的两种方式

 1. 编译时初始化
 2. 运行时初始化



# 关于关键字

关键字或保留字是语言中用于某些内部过程或表示某些预定义操作的单词
 因此，这些词不允许用作变量名或对象。这样做会导致编译时错误



> C# 关键字类别

 1. 值类型关键字：值类型中有15个关键字，用于定义各种数据类型
      ｜bool｜byte｜char｜decimal｜double｜enum｜float｜int｜long｜sbyte｜short｜struct｜unit｜ulong｜ushort

 2. 引用类型关键字：引用类型中有 6 个关键字，用于存储数据或对象的引用
      ｜class｜delegate｜interface｜object｜string｜void｜

 3. 修饰符关键字：修饰符中有 17 个关键字，用于修饰类型成员的声明
      ｜public｜private｜internal｜protected｜abstract｜const｜event｜extern｜new｜override｜
   ｜partial｜readonly｜sealed｜static｜unsafe｜virtual｜volatile｜

 4. 语句关键字：程序指令中使用的关键字共有 18 个
      ｜if｜else｜switch｜do｜for｜foreach｜in｜while｜break｜continue｜goto｜return｜throw｜
   ｜try｜catch｜finally｜checked｜unchecked｜

 5. 方法参数关键字：共有 4 个关键字，用于更改传递给方法的参数的行为
      ｜params｜in｜ref｜out｜

 6. 命名空间关键字：该类别共有 3 个用于命名空间的关键字
      ｜namespace｜using｜extern｜

 7. 运算符关键字：共有 8 个关键字，用于不同的目的，如创建对象、获取对象的大小等
      ｜as｜is｜new｜sizeof｜typeof｜true｜false｜stackalloc｜

8. 转换关键字：有 3 个关键字用于类型转换
   ｜explicit｜implicit｜operator｜

9. 访问关键字：有 2 个关键字用于访问和引用类或类的实例
    ｜base｜this｜

10. 文字关键字：有 2 个关键字用作文字或常量
   ｜null｜default｜
   
   

>  **要点**

关键字不用作类、变量等的标识符或名称
如果要使用关键字作为标识符，则必须使用 @ 作为前缀。例如，@abstract 是有效的标识符，但不是抽象的，因为它是关键字

 

# 标识符规则

标识符规则：
1. 标识符唯一允许的字符是所有字母数字字符（[A-Z]、[a-z]、[0-9]）、“_”（下划线）
2. 标识符不应以数字 ([0-9]) 开头
3. 标识符不应包含空格
4. 标识符不允许用作关键字，除非它们包含 @ 作为前缀
5. C# 标识符允许使用 Unicode 字符
6. C# 标识符区分大小写
7. C# 标识符不能包含超过 512 个字符
8. 标识符的名称中不包含两个连续的下划线，因为此类标识符用于实现