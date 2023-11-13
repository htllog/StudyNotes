# NSubstitute

NSubstitute 是一个用于 .NET 平台的模拟（ mocking ）框架。它允许开发者创建模拟对象，以在测试中替代真实的对象，从而验证代码的行为和测试特定的交互



## 作用

1. **模拟对象：** NSubstitute 允许您创建模拟对象，这些对象模拟了实际对象的行为。通过模拟对象，您可以隔离代码的不同部分，集中测试某一部分的逻辑，而不受其他部分的影响
2. **行为验证：** NSubstitute 提供了一套用于验证模拟对象的调用和行为的方法。您可以检查模拟对象的特定方法是否被调用，以及它们被调用时所传递的参数
3. **设置返回值：** 您可以使用 NSubstitute 设置模拟对象方法的返回值，以模拟不同的场景。这对于测试特定条件下的代码路径非常有用
4. **异常模拟：** NSubstitute 允许您模拟方法抛出异常的行为，以测试代码对异常情况的处理
5. **参数匹配：** NSubstitute 支持参数匹配，您可以使用 `Arg.Any<T>()` 等方法来表示任意类型的参数值，从而更灵活地验证模拟对象的调用
6. **异步支持：** NSubstitute 提供了对异步代码的支持，您可以轻松地模拟异步方法和进行异步测试

多结合 NUnit、xUnit 等一起使用，在单元测试中发挥重要作用



## 隔离框架

即字面意：用于隔离某个代码单元（方法、类、模块）工具，能够在独立、受控的环境中测试。隔离通常包括被测单元与其依赖项（例如其他类、方法、服务等）分离，以便测试被测单元的行为，隔绝外部环境的影响，尽量做到被 mock 的数据测试和实际一致



> 关键点

1. 模拟对象的创建：使用模拟对象隔离测试代码，在测试不依赖外部服务 or 组件
2. 行为验证：确保被测 code 按照期望效果与依赖项进行交互，确保代码在开发和维护周期中保持正确行为
3. 设置返回值：设置模拟对象方法的返回值，从而模拟不同的场景。使测试能够覆盖不同的代码路径，包括异常情况



## 示例

### demo

接口：

```c#
public interface ICalculator
{
    int Add(int a, int b);
  
    string GetDescription();
}
```



创建模拟对象，设置模拟对象行为：

```c#
using NSubstitute;

class Program
{
    static void Main()
    {
        var calculator = Substitute.For<ICalculator>();

        calculator.Add(1, 2).Returns(3);
        calculator.GetDescription().Returns("Eaxample"); 

        // 调用模拟对象的方法
        int result = calculator.Add(1, 2);
        string description = calculator.GetDescription();

        // 验证模拟对象的调用
        calculator.Received().Add(1, 2);
        calculator.Received().GetDescription();

        Console.WriteLine($"Result: {result}");
        Console.WriteLine($"Description: {description}");
    }
}
```



### Substitute.For<T>()

```c#
// 创建一个模拟的 ISomeInterface 对象
var mock = Substitute.For<ISomeInterface>();

// 设置模拟对象的行为
mock.SomeMethod().Returns("Test");

// 断言
Assert.AreEqual("Test", mock.SomeMethod());
```



### Test

> 定义图书信息和服务接口

```c#
public class Livro
{ 
    public string Titulo { get; set; }
    public string Autor { get; set; }
    public int AnoPublicacao { get; set; }
}

public interface IServicoCatalogoLivros
{ 
    IList<Livro> ConsultarCatalogo();
    Livro ConsultarLivroPorTitulo(string titulo); 
}
```



> 实现 `Biblioteca` 类，该类使用 `IServicoCatalogoLivros` 进行图书查询

```c#
public class Biblioteca 
{
    private readonly IServicoCatalogoLivros _servicoCatalogo;

    public Biblioteca(IServicoCatalogoLivros servicoCatalogo)
    {
        _servicoCatalogo = servicoCatalogo;
    }

    public bool LivroDisponivel(string titulo)
    {
        var catalogo = _servicoCatalogo.ConsultarCatalogo();
        var livro = _servicoCatalogo.ConsultarLivroPorTitulo(titulo);

        if (livro != null && catalogo.Contains(livro))
        {
            return true;
        }

        return false;
    }
}
```



> 定义测试用例

使用 NSubstitute 创建模拟对象，并对 `Biblioteca` 类的 `LivroDisponivel` 方法进行测试

```c#
public class Testes
{ 
    private const string TITULO_LIVRO_EXISTENTE = "Aprendendo C#"; 
    private const string TITULO_LIVRO_INEXISTENTE = "Código Limpo";
    
    private IServicoCatalogoLivros mockServicoCatalogo; 
    
    public Testes() 
    { 
        mockServicoCatalogo = Substitute.For<IServicoCatalogoLivros>();
        
        mockServicoCatalogo.ConsultarCatalogo().Returns(new List<Livro> 
        { 
            new Livro { Titulo = "Aprendendo C#", Autor = "John Doe", AnoPublicacao = 2020 }, 
            new Livro
            {
                Titulo = "Introdução ao ASP.NET Core", Autor = "Jane Doe", AnoPublicacao = 2019 
            }
        });
        
        mockServicoCatalogo.ConsultarLivroPorTitulo(TITULO_LIVRO_EXISTENTE)
            .Returns(new Livro { Titulo = TITULO_LIVRO_EXISTENTE, Autor = "John Doe", AnoPublicacao = 2020 }); 
        
        mockServicoCatalogo.ConsultarLivroPorTitulo(TITULO_LIVRO_INEXISTENTE).Returns((Livro)null);
    }

    [Fact]
    public void TestarLivroDisponivel_LivroExistente()
    { 
        var biblioteca = new Biblioteca(mockServicoCatalogo);

        // Act
        bool resultado = biblioteca.LivroDisponivel(TITULO_LIVRO_EXISTENTE);

        // Assert
        Assert.True(resultado);
    }

    [Fact]
    public void TestarLivroDisponivel_LivroInexistente()
    {
        // Arrange
        var biblioteca = new Biblioteca(mockServicoCatalogo);

        // Act
        bool resultado = biblioteca.LivroDisponivel(TITULO_LIVRO_INEXISTENTE);

        // Assert
        Assert.False(resultado);
    }
}
```

