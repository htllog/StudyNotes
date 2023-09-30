# 委托

委托可用于实现事件、回调函数和多播委托等功能

1. **委托基础**

   定义委托

   ```c#
   public delegate void MyDelegate(string message);
   ```

   创建委托实例

   ```c#
   MyDelegate delegateInstance = new MyDelegate(ShowMessage);
   ```

   委托调用

   ```c#
   delegateInstance("Hello, World!");
   ```

   委托方法

   ```c#
   public static void ShowMessage(string message)
   {
       Console.WriteLine("Message: " + message);
   }
   ```

   

2. **委托用法**

   事件处理

   ```c#
   public class EventPublisher
   {
       public delegate void EventHandler(string message);
       public event EventHandler OnEvent;
   
       public void RaiseEvent(string message)
       {
           if (OnEvent != null)
               OnEvent(message);
       }
   }
   
   public class EventSubscriber
   {
       public void Subscribe(EventPublisher publisher)
       {
           publisher.OnEvent += HandleEvent;
       }
   
       private void HandleEvent(string message)
       {
           Console.WriteLine("Event handled: " + message);
       }
   }
   ```

   

3. **匿名方法和 Lambda 表达式**

   匿名方法

   ```c#
   MyDelegate delegateInstance = delegate (string message)
   {
       Console.WriteLine("Anonymous method: " + message);
   };
   ```

   Lambda 表达式

   ```c#
   MyDelegate delegateInstance = (message) =>
   {
       Console.WriteLine("Lambda expression: " + message);
   };
   ```

   

> Example

```c#
using System;

namespace DelegateApplication
{
    // 声明一个接受 int 并返回字符串的方法的委托
    public delegate string MethodDelegate(int num);
    
    // 委托允许将方法作为参数传递，并在运行时调用动态调用这些方法
    public class DelegateDemo
    {
        // 定义委托可以指向的一些方法
        public class SampClass
        {
            // 定义实例方法
            public string StringMethod(int num)
            {
                if (num > 0)
                    return ("positive");
                if (num < 0)
                    return ("negative");
                return ("zero");
            }
        
            // 定义静态方法
            public static string SignMethod(int num)
            {
                if (num > 0)
                    return ("+");
                if (num < 0)
                    return ("-");
                return ("");
            }
        }
        
        public static void RunDelegateDemo()
        {
            // 为每一个方法创建一个委托，对于实例方法，必须提供实例，对于静态方法，使用 class name
            SampClass sampClass = new SampClass();
            MethodDelegate methodDelegate1 = new MethodDelegate(sampClass.StringMethod);
            MethodDelegate methodDelegate2 = new MethodDelegate(SampClass.SignMethod);
            
            Console.WriteLine("{0} is {1}; use the sign \"{2}\".", 5, methodDelegate1(5),methodDelegate2(5));
            Console.WriteLine( "{0} is {1}; use the sign \"{2}\".", -3, methodDelegate1( -3 ), methodDelegate2( -3 ) );
            Console.WriteLine( "{0} is {1}; use the sign \"{2}\".", 0, methodDelegate1( 0 ), methodDelegate2( 0 ) );
        }
    }
}
```



> 打印结果

```markdown
5 is positive; use the sign "+".
-3 is negative; use the sign "-".
0 is zero; use the sign "".
```



# 委托的优点和用途

1. **灵活性和扩展性：** 使用委托可以将方法作为参数传递，这增强了程序的灵活性。你可以在运行时决定调用哪个具体方法，而不必在编译时确定
2. **解耦和分离关注点：** 委托可以帮助解耦代码，使调用方与具体实现分离开来。调用方只需要关心委托的调用，而不需要知道具体实现细节，这有助于分离关注点，提高代码的可维护性和可读性
3. **支持多播委托：** 委托支持多播，即一个委托可以同时指向多个方法。这在事件处理等场景下非常有用，允许多个方法响应同一个事件

在上述代码中，通过委托将方法作为参数传递，可以在 `RunDelegateDemo` 方法中动态调用不同的方法，而不需要直接在该方法内部调用具体的方法。这样做使得代码更灵活、可扩展，并支持解耦和分离关注点的编程原则



# 多播委托

多播委托是一种特殊类型的委托，它可以关联多个方法，使得委托可以调用多个方法序列。这意味着当你调用多播委托时，它将按照关联方法的顺序依次调用每个关联方法，多播委托是通过使用 `+=` 运算符将方法附加到委托上来实现的

* 使用 `+=` 可以将一个或多个方法附加到委托上
* 使用 `-=` 可以从委托中移除一个或多个方法



```c#
using System;

namespace DelegateApplication
{
    // 多播委托
    public delegate void MulticastDelegate();
    
    public class MulticastDelegateExample
    {
        private static void Method1()
        {
            Console.WriteLine("Method1");
        }
        
        private static void Method2()
        {
            Console.WriteLine("Method2");
        }
        
        private static void Method3()
        {
            Console.WriteLine("Method3");
        }
        
        public static void RunMulticastDelegateDemo()
        {
            MulticastDelegate multicastDelegate = Method1;
            multicastDelegate += Method2;
            multicastDelegate += Method3;
            
            Console.WriteLine("Calling multicast delegate: ");
            
            // 调用多播委托
            multicastDelegate();
            
            // 移除一个方法
            multicastDelegate -= Method2;

            Console.WriteLine("\nCalling multicast delegate after removing a method: ");
            
            // 再次调用多播委托，但不包括已移除的方法
            multicastDelegate();  
        }
    }
}
```



> 打印结果

```markdown
Calling multicast delegate: 
Method1
Method2
Method3

Calling multicast delegate after removing a method: 
Method1
Method3
```



> Tips

Code 中，多播委托 MulticastDelegate 是一个没有返回值的委托，因此调用它不会返回结果。在这种情况下，多播委托是用于执行一系列操作，而不是返回值

总结来说，多播委托可以按顺序执行关联的多个方法，但不会返回结果，它主要用于执行一系列操作



# 调用 Method and Delegat

```c#
using System;

namespace DelegateApplication
{
    // 多播委托
    public delegate void MulticastDelegate();
    
    public class MulticastDelegateExample
    {
        private static void Method1()
        {
            Console.WriteLine("Method1");
        }
        
        private static void Method2()
        {
            Console.WriteLine("Method2");
        }
        
        private static void Method3()
        {
            Console.WriteLine("Method3");
        }
        
        public static void RunMulticastDelegateDemo()
        {
            MulticastDelegate multicastDelegate = Method1;
            multicastDelegate += Method2;
            multicastDelegate += Method3;
            
            Console.WriteLine("Calling multicast delegate: ");
            
            // 调用多播委托
            multicastDelegate();
            
            // 移除一个方法
            multicastDelegate -= Method2;

            Console.WriteLine("\nCalling multicast delegate after removing a method: ");
            
            // 再次调用多播委托，但不包括已移除的方法
            multicastDelegate();  
        }
    }
}
```



> 打印结果

```markdown
Direct method calls:
Double: 20
Increment: 11

Delegate method call (multicast):
Result: 11
```

调用多播委托方法时，最后 result 为 11 是因为：**多播委托调用时，只会返回最后一个关联方法的结果。**如果想得到每个方法的结果，可以通过遍历多播委托链并并分别调用每个方法，然后输出每个方法的结果，修改  `DelegateMethodCall`  方法如下：

```c#
public static void DelegateMethodCall()
{
    ThanDelegate thanDelegate = MathOperations.Double;
    thanDelegate += MathOperations.Increment;

    Console.WriteLine("\nDelegate method call (multicast):");
    
    // 遍历多播委托的委托链并逐个调用方法
    foreach (ThanDelegate method in thanDelegate.GetInvocationList())
    {
        int result = method(10);
        Console.WriteLine("Result: " + result);
    }
}
```

现在调用 `DelegateMethodCall` 方法会输出每个关联方法的结果



# 委托构造函数

创建委托实例时，委托构造函数可以接受不同类型的参数，包括方法、匿名方法和 lambda 表达式

```c#
using System;

namespace DelegateApplication
{
    // 定义一个委托类型
    public delegate void DelegateType(string msg);
    
    public class DelegateConstructors
    {
        public void DisplayMessage(string message)
        {
            Console.WriteLine("DisplayMessage: " + message);
        }
        
        private static void ShowMessage(string message)
        {
            Console.WriteLine("ShowMessage: " + message);
        }

        public static void RunDelegateApplication()
        {
            // 创建一个委托实例并关联到 DisplayMessage Method
            DelegateType delegate1 = new DelegateType(new DelegateConstructors().DisplayMessage);

            // 使用静态方法关联委托
            DelegateType delegate2 = new DelegateType(ShowMessage);

            // 使用匿名方法关联委托
            DelegateType delegate3 = delegate(string msg)
            {
                Console.WriteLine("Anonymous Method: " + msg);
            };

            // 使用 lambda 表达式关联委托
            DelegateType delegate4 = (msg) => Console.WriteLine("Lambda Expression: " + msg);

            // 调用委托关联的方法
            delegate1("Hello, using instance method.");
            delegate2("Hello, using static method.");
            delegate3("Hello, using anonymous method.");
            delegate4("Hello, using lambda expression.");
        }
    }
}
```



# 委托用法

## 使用委托实现异步操作

```c#
using System;

public class AsyncOperation
{
    public delegate void AsyncCallback(string result);

    public void PerformAsyncTask(AsyncCallback callback)
    {
        System.Threading.ThreadPool.QueueUserWorkItem((state) =>
        {
            string result = "异步操作完成";
            callback(result);
        });
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        AsyncOperation asyncOperation = new AsyncOperation();

        AsyncOperation.AsyncCallback callback = (result) =>
        {
            Console.WriteLine("调用回调. 结果: " + result);
        };

        asyncOperation.PerformAsyncTask(callback);

        Console.WriteLine("异步操作启动");
        Console.ReadLine();
    }
}

```



## 使用委托实现事件处理

```c#
using System;

public class EventPublisher
{
    public delegate void EventHandler(string message);

    public event EventHandler EventOccurred;

    public void SimulateEvent(string message)
    {
        Console.WriteLine("Event simulated: " + message);
        EventOccurred?.Invoke(message);
    }
}

public class EventSubscriber
{
    public void Subscribe(EventPublisher publisher)
    {
        publisher.EventOccurred += HandleEvent;
    }

    private void HandleEvent(string message)
    {
        Console.WriteLine("Event handled: " + message);
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        EventPublisher publisher = new EventPublisher();
        EventSubscriber subscriber = new EventSubscriber();

        subscriber.Subscribe(publisher);

        publisher.SimulateEvent("Event 1");

        Console.ReadLine();
    }
}
```



## 使用委托实现 LINQ 查询

```c#
using System;
using System.Linq;
using System.Collections.Generic;

public class LINQExample
{
    public delegate bool FilterDelegate(int num);

    public void FilterNumbers(List<int> numbers, FilterDelegate filter)
    {
        var result = numbers.Where(num => filter(num));
        Console.WriteLine("Filtered numbers: " + string.Join(", ", result));
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

        LINQExample linqExample = new LINQExample();

        LINQExample.FilterDelegate filter = (num) => num % 2 == 0;

        linqExample.FilterNumbers(numbers, filter);

        Console.ReadLine();
    }
}
```

