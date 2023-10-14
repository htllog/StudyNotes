# EntityFrameworkCore 

## 数据抽象层概念 Data Abstraction Layer

数据抽象层（数据访问层）作为计算机科学概念，用于隐藏数据的底层细节。用于各级应用程序业务逻辑（或应用层）和底层数据存储（通常为数据库）之间，提供一种抽象的方式来访问和操作数据。数据抽象层的目的是降低数据访问的复杂性，提供一种规范化的方式来执行 CRUD 操作（创建、读取、更新、删除）以及其他与数据相关的操作

数据抽象层**主要功能**包括：

1. 隔离数据存储
2. 提供数据访问接口
3. 支持事务管理
4. 性能优化
5. 跨数据库兼容性
6. 安全性和数据验证

实际应用中，数据抽象层通常由数据访问框架（例如 Entity Framework 、 Hibernate 、 Django ORM 等）来实现。这些框架提供了数据访问的标准方式，简化了数据访问代码的编写，并增加了应用程序的可维护性和扩展性。数据抽象层对于大多数现代应用程序都至关重要，特别是涉及数据库或其他数据存储的应用程序



## ORM

ORM（ Object-Relational Mapping ），用于将对象模型和关系数据库模型进行映射，从而在程序中以面向对象的方式操作数据库，而不必直接使用 SQL 语句

简单来说，ORM 允许开发者使用面向数据库的方式来数据库，从而深入了解数据库的底层结构和 SQL 语言。每个对象对应数据库中的一条记录，对象的属性对应数据库记录的字段

> 优点

1. 提高开发效率
2. 减少错误
3. 提高代码可读性



> 缺点

1. 性能问题

    ORM 框架为了提高开发效率，做了很多封装，但这些封装可能会增加系统的开销，如果使用不当，可能会导致性能问题

2. 过度依赖

   过度依赖 ORM 框架，开发者可能会忽视底层的数据库知识，当遇到复杂的数据库问题时，可能会束手无策

3. 不适合复杂的数据库操作

    对于一些复杂的数据库操作，ORM 框架可能无法满足需求，还需要开发者手动编写 SQL 语句



> 核心

* 数据映射

  将程序中的对象映射到数据库中的数据，实现了两者的关联

* 实体操作

  包括增删改查，通过操作实体对象来操作数据库

* 查询语言

  可以通过面向对象的方式来查询数据库

* 事务处理

  实现事务的提交和回滚



## EF Core

Entity Framework Core ( EF Core ) 是对象-关系映射程序 ( ORM )。 ORM 在代码和数据库中实现的域模型之间提供一个层。 EF Core 是一种数据访问 API ，它允许开发者使用 .NET 普通的公共运行时语言（ CLR ）对象（通常称为 POCO，Plain Old CLR Objects ）来表示数据库中的实体或表。开发者可以使用强类型语言的集成查询语法 LINQ 与这些 CLR 对象进行交互，实现对数据库的查询、插入、更新、删除等操作。这种交互方式使得操作数据库更加直观、简洁和类型安全

在 EF Core 中，数据库可在 .NET POCO 后面抽象化。 EF Core 处理与基础数据库的直接交互。 使用此 API 时，可以花更少的时间来与数据库之间转换请求以及编写 SQL，从而让你有更多时间专注于重要的业务逻辑，使用 EF Core 可以：

* 将数据作为 C# 对象（实体）加载
* 通过对实体调用这些方法来添加、修改和删除数据
* 将多个数据库表映射到单个 C# 实体
* 处理多个用户同时尝试更新同一记录时出现的并发问题
* 使用强类型 LINQ ([ System.Linq ](https://learn.microsoft.com/zh-cn/dotnet/api/system.linq)) 语法来查询数据库
* 访问[多种数据库](https://learn.microsoft.com/zh-cn/ef/core/providers/)，包括 SQL Server、SQLite、Azure Cosmos DB、PostgreSQL、MySQL 等
* 从现有数据库生成域模型
* 根据域模型管理数据库架构
* 使用单个方法调用提交对相关实体的复杂、深层和/或宽对象图的更改



## DbContext

### 实现

1. **安装 Entity Framework Core 所需的包**

   ```bash
   dotnet add package Microsoft.EntityFrameworkCore
   dotnet add package Microsoft.EntityFrameworkCore.Design
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   ```

   

2. **创建数据库上下文**

   ```c#
   public class ApplicationDbContext : DbContext
   {
       public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
       {
       }
       
       // 为每个实体定义 DbSet 属性
       // public DbSet<MyEntity> MyEntities { get; set; }
   }
   ```

   

3. **启动中配置 DbContext**

   在 Startup.cs 中 `ConfigureServices` 方法配置 DbContext

   ```c#
   using Microsoft.EntityFrameworkCore;
   
   public void ConfigureServices(IServiceCollection services)
   {
       services.AddDbContext<AppDbContext>(options =>
       {
           options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection"));
       });
   }
   ```



### 关键

1. 数据库交互

   DbContext 提供了一种使用高级方法与数据库交互的方式，允许您执行 CRUD（创建、读取、更新、删除）操作

2. 实体跟踪

   DbContext 跟踪内存中实体的更改。从数据库检索实体时，它们就会被跟踪。调用 SaveChanges 方法时，对这些跟踪实体所做的任何更改都可以保留到数据库中

3. 工作单元

   DbContext 充当工作单元，允许执行一系列操作，然后使用 SaveChanges 一次提交所有操作。有助于保持数据库的一致性和完整性

4. 实体关系

   DbContext 知道实体之间的关系，您可以使用 Fluent API 或数据注释来定义这些关系

5. 配置

   可以使用各种配置来配置 DbContext 及其实体的行为，例如指定数据库提供程序、定义实体映射等

6. 事务管理

   DbContext 允许你管理事务，使你能够控制操作的原子性



## 数据注解和模型映射

模型映射和数据注释是 Entity Framework Core ( EF Core ) 中用于定义 C# 类（模型）和数据库表之间映射的两种方法。这两种方法的目的都是将 C# 类的属性映射到数据库表中的相应列

1. 数据注释

   数据注释是应用于 C# 类的属性的属性。这些属性向 EF Core 提供元数据，用于定义基于 C# 类的数据库架构。以下是一些常用的数据注释：

   - `[Key]`：指定实体的主键
   - `[Required]`：表示该属性是必需的（不可为空）
   - `[MaxLength]`：指定字符串属性的最大长度
   - `[Column]`：指定数据库列名称和其他属性（例如类型）

   

   ```c#
   public class Product
   {
       [Key]
       public int ProductId { get; set; }
       
       [Required]
       [MaxLength(100)]
       public string Name { get; set; }
       
       [Column(TypeName = "decimal(18, 2)")]
       public decimal Price { get; set; }
   }
   ```

   

2. 模型映射 Fluent API

   Fluent API 是数据注释的替代方案，您可以在其中配置模型映射和关系`OnModelCreating` 你的方法`DbContext`. 它提供了对数据库模式和关系的更高级和更细粒度的控制

   ```c#
   protected override void OnModelCreating(ModelBuilder modelBuilder)
   {
       modelBuilder.Entity<Product>()
           .HasKey(p => p.ProductId);
   
       modelBuilder.Entity<Product>()
           .Property(p => p.Name)
           .IsRequired()
           .HasMaxLength(100);
   
       modelBuilder.Entity<Product>()
           .Property(p => p.Price)
           .HasColumnType("decimal(18, 2)");
   }
   ```



数据注解更易于使用和理解，特别是对于简单的映射。另一方面，Fluent API 提供了更多的控制和灵活性，在复杂的场景或当您需要定义更高级的映射和关系时，可以使用



TIps：

```c#
    // 使用数据注解，不需要在 OnModelCreating 方法中使用 Fluent API 进行配置。这是因为 EF Core 将根据数据注解自动推断模型的配置
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // modelBuilder.Entity<Product>().HasKey(p => p.ProductId);
        // modelBuilder.Entity<Product>().Property(p => p.Name).IsRequired().HasMaxLength(100);
        // modelBuilder.Entity<Product>().Property(p => p.Price).HasColumnType("decimal(18, 2)");
    }
```



## Repository Pattern Overview 存储库（仓储）模式

存储库模式是一种设计模式，它将从数据库（或任何数据源）检索数据的逻辑与应用程序的业务逻辑分开。它通过抽象数据访问并隐藏数据源的详细信息，提供了清晰的关注点分离

**仓储服务**是数据访问层的抽象呈现。它隐藏了如何从底层数据源保存或查询数据的详细信息。有关如何存储和查询数据的详细信息，有关如何保存和查询数据的详细信息都在对应的仓储中

### 概念

1. 存储库接口：定义数据访问操作的契约。它通常包括 GetById、GetAll、Add、Update 和 Delete 等方法
2. 具体存储库：实现存储库接口并提供数据访问的实际实现
3. 模型/实体：表示与存储库交互的数据结构或对象
4. 工作单元：可选 - 一个更高级别的概念，表示涉及多个存储库的事务或工作单元

### 使用

1. 定义存储库接口：首先定义一个接口，该接口声明常见数据操作（如 Get、GetAll、Add、Update 和 Delete）的方法
2. 创建具体存储库：在具体存储库类中实现存储库接口。这些类应该包含实际的数据访问逻辑
3. 依赖注入：使用依赖注入容器将存储库实例注入到您的应用程序中
4. 在控制器或服务中的使用：使用应用程序的控制器或服务中的存储库与数据源进行交互
5. 关注点分离：记住，目标是将数据访问的关注点与其他应用程序逻辑分开。确保存储库仅涉及数据访问

