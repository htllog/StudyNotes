# 关于静态 or 非静态

在面向对象编程中，"静态"（static）和"非静态"（non-static）是两种成员修饰符，用于定义类的成员（方法、变量等）的特性和行为。这些特性决定了如何使用和调用这些成员



# 静态成员 (static)

- 静态成员属于类级别，而不是实例级别。它们在程序启动时被初始化，不需要依赖于类的实例来使用
- 可以通过类名直接访问静态成员，不需要创建类的实例
- 静态成员只有一份，所有实例共享同一个静态成员
- 静态成员常用于表示与整个类相关的特性，如共享数据、工具方法等



#  非静态成员 (non-static)

- 非静态成员属于类的实例级别，每个类的实例都有自己的一份非静态成员
- 需要先创建类的实例，然后通过实例来访问非静态成员
- 非静态成员可以访问类的静态成员和非静态成员
- 非静态成员常用于表示实例特定的属性、行为等



# 区别总结

- **作用域和生命周期**：静态成员的作用域为整个类，生命周期与应用程序相同；非静态成员的作用域限定在实例内，生命周期与实例的生命周期相同
- **访问方式**：静态成员可以通过类名直接访问，非静态成员需要先创建类的实例
- **数据共享**：静态成员是共享的，所有实例共用一份；非静态成员是每个实例独有的
- **使用场景**：静态成员常用于表示类级别的特性，如共享数据、工具方法；非静态成员常用于表示实例特定的属性和行为



# Example

```c#
public class ExampleClass
{
    // 静态变量
    public static int StaticCounter { get; set; }

    // 非静态变量
    public int InstanceCounter { get; set; }

    // 静态方法
    public static void StaticMethod()
    {
        Console.WriteLine("This is a static method.");
    }

    // 非静态方法
    public void InstanceMethod()
    {
        Console.WriteLine("This is an instance method.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // 访问静态成员
        ExampleClass.StaticCounter = 10;
        ExampleClass.StaticMethod();

        // 创建类的实例
        ExampleClass instance1 = new ExampleClass();
        ExampleClass instance2 = new ExampleClass();

        // 访问非静态成员
        instance1.InstanceCounter = 20;
        instance2.InstanceCounter = 30;

        instance1.InstanceMethod();
        instance2.InstanceMethod();

        // 输出静态和非静态成员的值
        Console.WriteLine("Static counter: " + ExampleClass.StaticCounter);
        Console.WriteLine("Instance counter for instance1: " + instance1.InstanceCounter);
        Console.WriteLine("Instance counter for instance2: " + instance2.InstanceCounter);
    }
}
```

