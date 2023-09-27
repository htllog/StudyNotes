# Access attributes using reflection 使用反射访问属性

反射允许程序检查自身、发现类型并与它们交互。 反射的关键方面包括：

1. 获取类型信息： 您可以使用 typeof 运算符或在实例上使用 GetType() 方法来获取有关类型（类、接口、结构、枚举）的信息

   ```c#
   Type type = typeof(MyClass);
   ```

2. 访问成员： 反射允许检查类型的成员（字段、属性、方法）并与之交互

3. 动态创建实例： 可以使用 Activator.CreateInstance 方法动态创建类型的实例

4. 调用方法：
   反射允许您使用 Invoke 方法动态调用方法

5. 访问字段和属性： 您可以使用反射动态获取和设置字段或属性值

   

# C# 如何使用反射

以下为 C# 中使用反射分步指南：

1. 访问类型信息： 使用 `typeof` 或 `GetType()`
2. 创建实例： 使用 `Activator.CreateInstance`
3. 调用方法： 使用 `GetMethod` 获取方法并使用 `Invoke` 调用它
4. 访问字段和属性：
   使用 `GetField` 或 `GetProperty` 获取字段或属性信息，然后使用 `GetValue` 和 `SetValue` 获取或设置值



> 分步指南

1. 创建一个类，例如`Calculator`，该类包含一个可以调用的方法

   ```c#
   public class Calculator
   {
       public int Add(int a, int b)
       {
           return a + b;
       }
   }
   ```

2. 在Main方法中使用反射调用该类的方法

   ```c#
   using System;
   using System.Reflection;
   
   class Program
   {
       static void Main(string[] args)
       {
           // 步骤 1: 获取 Calculator 类型信息
           Type type = typeof(Calculator);
   
           // 步骤 2: 创建 Calculator 实例
           object calculatorInstance = Activator.CreateInstance(type);
   
           // 步骤 3: 获取 Add 方法信息
           MethodInfo method = type.GetMethod("Add");
   
           // 步骤 4: 调用 Add 方法
           int result = (int)method.Invoke(calculatorInstance, new object[] { 10, 20 });
   
           // 步骤 5: 显示结果
           Console.WriteLine("Result of Add method: " + result);
       }
   }
   ```

   

> Example: Invoking a Method Using Reflection

```c#
using System;
using System.Reflection;

namespace ReflectionApplication
{
    internal class Program
    {
        public class MyClass
        {
            public int Add(int a, int b)
            {
                return a + b;
            }

            public string Concatenate(string str1, string str2)
            {
                return str1 + str2;
            }
        }

        public static void Main(string[] args)
        {
            // 使用反射获取 MyClass 类型
            Type myClassType = typeof(MyClass);
            
            // 使用反射创建 MyClass 实例
            object myClassInstance = Activator.CreateInstance(myClassType);
            
            // 调用 Add 方法 -- 使用反射来获取有关 MyClass 中的 Add 方法的信息
            MethodInfo addMethod = myClassType.GetMethod("Add");
            // 使用反射调用 MyClass 实例上的 Add 方法，传递参数捕获结果
            int result = (int)addMethod.Invoke(myClassInstance, new object[] { 10, 20 });
            Console.WriteLine("Add result: " + result);
            
            // 调用 Concatenate 方法
            MethodInfo concatenateMethod = myClassType.GetMethod("Concatenate");
            string concatResult = (string)concatenateMethod.Invoke(myClassInstance, new object[] { "Hello ", "Htl" });
            Console.WriteLine("Concatenate result: " + concatResult);
        }
    }
}
```



> C# defines custom properties and retrieves examples through reflection

C#定义自定义属性，将其应用于多个实体，并通过反射检索例子

1. 定义自定义属性类

   ```c#
   public class Person
   {
     	// MyCustom 更改此设置以使用您的自定义属性“自定义”
       [MyCustom("This is the name property.")]
       public string Name { get; set; }
   
       [MyCustom("This is the age property.")]
       public int Age { get; set; }
   }
   ```

   

2. 使用反射检索属性及其值

```c#
Type type = typeof(Person);

foreach (var propertyInfo in type.GetProperties())
{
    MyCustomAttribute customAttribute =
        (MyCustomAttribute)Attribute.GetCustomAttribute(propertyInfo, typeof(MyCustomAttribute));

    if (customAttribute != null)
    {
        Console.WriteLine($"Property: {propertyInfo.Name}");
        Console.WriteLine($"Description: {customAttribute.Description}");

        // 根据需要访问和使用属性的属性
    }
}
```



> Example

```c#
using System;

namespace ReflectionApplication
{
    internal class Program
    {
        /*
         * 自定义特性
         * AttributeTargets.Property：
         * 此参数指定自定义属性 CustomAttribute 可以应用于属性可以将自定义属性应用于各种程序元素，例如类、方法、属性等
         * 在这种情况下，它仅限于属性
         *
         * Inherited = false：该参数指定自定义属性是否由派生类继承。本例中，设置为 false，这意味着自定义属性不会被派生类继承
         *
         * AllowMultiple = false：此参数指定是否可以将自定义属性多次应用于同一目标。将其设置为 false 意味着该属性只能应用于属性一次
         */
        [AttributeUsage(AttributeTargets.Property, Inherited = false, AllowMultiple = false)]
        // sealed class MyCustomAttribute : Attribute 声明一个名为 CustomAttribute 的自定义属性，属性继承 Attribute
        sealed class CustomAttribute : Attribute 
        {
            // Description 可以保存附加信息的自定义属性的属性
            public string Description { get; }

            // 初始化属性的构造函数Description
            public CustomAttribute(string description)
            {
                Description = description;
            }
        }
        
        // 应用自定义属性到类的成员
        public class Person
        {
            /*
             * C# 中，使用自定义属性时省略 Attribute 后缀是一种约定。例如，CustomAttribute 通常用作 Custom。为了简洁性和可读性，C# 编译器允许这样做
             * Custom 是属性使用的 CustomAttribute 的简写形式，使代码更加简洁和可读
             */
            [CustomAttribute("This is the name property.")]
            public string Name { get; set; }

            [Custom("This is the age property.")]
            public int Age { get; set; }
        }


        public static void Main(string[] args)
        {
            // 使用引用搜索属性及其值，获取 Person 类的 Type 对象，typeof(Person) 返回表示 Person 类的 Type 对象
            Type type = typeof(Person);

            // 循环，迭代 Person 类的属性
            foreach (var propertyInfo in type.GetProperties())
            {
                // 检索应用于该属性的 CustomAttribute 类型的任何自定义特性
                // 检索应用于属性的指定类型（本例中为 CustomAttribute）的自定义特性数组
                var customAttributes = propertyInfo.GetCustomAttributes(typeof(CustomAttribute), false);

                // 检查是否找到当前属性的任何自定义属性
                if (customAttributes.Length > 0)
                {
                    // 找到自定义属性，此行会将第一个自定义属性转换为 CustomAttribute 类型
                    var customAttribute = (CustomAttribute)customAttributes[0];
                    // 打印属性名称
                    Console.WriteLine($"Property: {propertyInfo.Name}");
                    // 打印属性描述
                    Console.WriteLine($"Description: {customAttribute.Description}");
                }
            }
        }
    }
}
```



