# AutoFac

## 概念

- **依赖注入（DI）**

  依赖注入是一种设计模式，用于将类的依赖关系从类本身中分离出来，以便更好地管理和测试这些依赖关系

- **容器（Container）**

  容器是用于管理和解决类之间依赖关系的工具。AutoFac 就是一个依赖注入容器

- **注册（Registration）**

  在 AutoFac 中，注册是指将类型或实例与接口或基类关联起来，以便容器能够创建和解析这些类型的实例



## 常见生命周期设置

Autofac 最常见的生命周期设置是

- 单实例
- 每个依赖项的实例
- 每个生命周期范围的实例



### 单实例

对于单实例生命周期，容器中最多有一个组件实例，并且当它注册的容器被释放时（例如 `appContainer` 上面），它将被释放

- 每次解析时创建一个实例，并在随后的每次解析中返回相同的实例
- 适用于全局唯一且无状态的组件，确保唯一性并节省资源

使用修饰符将组件配置为具有此生命周期 `SingleInstance()` ：

```c#
builder.Register(c => new MyClass()).SingleInstance();
```



每次从容器请求这样的组件时，都会返回相同的实例:

```c#
var a = container.Resolve<MyClass>();
var b = container.Resolve<MyClass>();
Assert.AreSame(a, b);
```



### 每个依赖项的实例

当组件注册中未指定生命周期设置时，将假定每个依赖项都有一个实例。每次从容器请求这样的组件时，都会创建一个新实例

- 每次解析或请求时创建一个新的实例
- 适用于短暂、无状态的组件，每次解析时都创建一个新对象

```c#
var a = container.Resolve<MyClass>();
var b = container.Resolve<MyClass>();
Assert.AreNotSame(a, b);
```



### 每个生命周期范围的实例

- 每个生命周期范围（通常是HTTP请求或作用域）中创建一个实例，同一生命周期范围内解析得到的是同一个实例
- 适用于特定生命周期范围内共享的组件，如 Web 应用程序中的每个 HTTP 请求或自定义生命周期

最终的基本生命周期模型是 per-lifetime-scope，使用 `InstancePerLifetimeScope()` 修饰符实现:

```c#
builder.Register(c => new MyClass()).InstancePerLifetimeScope();
```



# IoC

## 控制反转原则
![image](https://github.com/htllog/StudyNotes/assets/118370026/8be7e77e-a07f-48de-b971-69f508848df0)


通过上图可以观察到：

- 聚合其他类的主类不应依赖于聚合类的直接实现。这两个类都应该依赖于抽象。因此，客户类别不应直接依赖于地址类别。地址类和客户类都应该依赖于使用接口或抽象类的抽象
- 抽象不应该依赖于细节，细节应该依赖于抽象

因此控制反转原则可以归纳为：

1. **依赖倒置原则**（Dependency Inversion Principle，DIP）

   - 高层模块不应该依赖于低层模块，它们都应该依赖于抽象
   - 抽象不应该依赖于细节，细节应该依赖于抽象

   这意味着我们应该通过抽象来定义系统的结构，而具体的实现应该依赖于抽象的定义

2. **单一职责原则**（Single Responsibility Principle，SRP）

   - 一个类应该只有一个引起变化的原因

   这个原则强调一个类应该有清晰的职责，避免臃肿复杂的类，使得类更加可维护、可理解和可测试



## IoC 框架
![image](https://github.com/htllog/StudyNotes/assets/118370026/c48c5c17-8ba2-4f2b-9787-b0bf42d66c33)


图 IoC 框架显示了我们如何实现这种解耦。最简单的方法是公开一个允许我们设置对象的方法。将地址对象的创建委托给 IoC 框架。IoC 框架可以是类、客户端或某种 IoC 容器。因此，IoC 框架创建地址对象并将此引用传递给客户类可将分为两步 Step1 and Step2 



### Autofac体现

创建接口和接口所需实现类：

```c#
public interface IMessageService
{
    string GetMessage();
}

public class EmailService : IMessageService
{
    public string GetMessage()
    {
        return "This is an email message.";
    }
}

public class SMSService : IMessageService
{
    public string GetMessage()
    {
        return "This is an SMS message.";
    }
}
```



程序入口点注册：

```c#
using Autofac;
using System;

public class Program
{
    static void Main(string[] args)
    {
        // 创建容器构建器
        var builder = new ContainerBuilder();

        // 注册接口和实现类
        builder.RegisterType<EmailService>().As<IMessageService>();
        // 注册另一个实现类
        builder.RegisterType<SMSService>().As<IMessageService>();

        // 构建容器
        var container = builder.Build();

        // 从容器中解析出实现类
        var emailService = container.Resolve<IMessageService>();

        // 调用实现类的方法
        Console.WriteLine(emailService.GetMessage());

        // 输出：
        // This is an email message.
    }
}
```



上述 demo 中，Autofac 体现了控制反转（ IoC ）原则的几个关键特点：

1. **依赖注入**

   Autofac 通过注册接口与实现类的关联，在需要使用服务时自动注入依赖。在 Main 方法中，我们只关心解析 `IMessageService` 接口，而不需要关心其具体实现类。这种依赖的注入方式称为依赖注入，它是 IoC 的一种具体实现

   ```c#
   builder.RegisterType<EmailService>().As<IMessageService>();
   builder.RegisterType<SMSService>().As<IMessageService>();
   ```

   

2. **控制反转**

   控制反转意味着将对象的创建和管理权交给外部容器（ Autofac ），而不是由代码直接创建。传统模式中，会在 Main 方法中直接创建 `EmailService` 或 `SMSService` 的实例，但在 Autofac 通过容器解析实例，实现了对服务的控制反转

   ```c#
   var emailService = container.Resolve<IMessageService>();
   ```

   

3. **解耦合**

   通过依赖注入，将接口与实现类解耦合。Main 方法只依赖于 `IMessageService` 接口，不关心具体的实现类。这种解耦合使得代码更具灵活性和可维护性



Autofac 的使用体现了 IoC 原则，在于体现将对象的创建和依赖的解析交给了容器，降低了代码之间的耦合度，使得系统更灵活、易于扩展和维护

传统模式中，对象的创建和依赖解析由代码直接控制，在 IoC 容器中，这些控制被反转，由容器负责管理对象的创建和依赖的注入

