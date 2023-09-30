# 封装 (Encapsulation)

封装是将数据（字段）和操作数据的方法（函数）绑定在一起的机制。封装通过将数据隐藏在类的内部，只允许通过受控的方式访问数据，以确保数据的安全性和有效性

> Example

```c#
public class BankAccount
{
    private decimal balance;

    public void Deposit(decimal amount)
    {
        // 简单的封装，限制了对 balance 的直接访问
        balance += amount;
    }

    public decimal GetBalance()
    {
        // 通过方法提供对 balance 的访问
        return balance;
    }
}
```



# 抽象 (Abstraction)

抽象是一种将复杂系统简化为其关键特征的过程，以便更好地理解和处理系统。它隐藏了系统的实现细节，只显示对于当前上下文重要的内容

**数据抽象是一种仅向用户展示基本细节的属性。琐碎或非必要的单元不会展示给用户**，对象的属性和行为将其与类似类型的其他对象区分开来，并且还有助于对对象进行分类/分组

```c#
public abstract class Shape
{
    public abstract double Area(); // 抽象方法，用于计算面积
}

public class Circle : Shape
{
    private double radius;

    public Circle(double r)
    {
        radius = r;
    }

    public override double Area()
    {
        return Math.PI * radius * radius;
    }
}
```



# 区别

- 封装是将相关的数据和方法组合在一起，以确保数据的安全性和有效性。
- 抽象是隐藏不必要的细节，突出重要特征，以便更好地理解和处理系统。抽象可以用于类、方法、接口等
- **封装一般用于对数据操作，比如实体，抽象一般抽象类抽象方法**
