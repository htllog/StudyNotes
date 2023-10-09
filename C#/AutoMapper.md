# AutoMapper

> 概念

1. 映射配置 Mapping Configuration

   AutoMapper 需要配置来定义一种类型的属性应如何映射到另一种类型的属性。这通常在 AutoMapper 配置文件中完成

2. 配置文件 Profiles

   AutoMapper 配置文件是您定义类型之间映射的类。每个配置文件通常对应于特定的映射场景

3. 映射 Mapping

   AutoMapper 使用配置文件和配置来执行对象之间的映射。它根据源属性的名称和类型将源属性与目标属性进行匹配



> 入门

1. 安装

   通过 NuGet Package Manager 或 dotnet CLI 安装 AutoMapper 包

   ```bash
   dotnet add package AutoMapper
   ```

2. 创建映射配置文件

   创建一个扩展配置文件的类来定义映射。配置文件是您定义一个类的属性如何映射到另一类的属性的位置

   `CreateMap` 方法有两个参数，第一个参数是源类型，第二个参数是目标类型

   ```c#
   using AutoMapper;
   
   public class MappingProfile : Profile
   {
       public MappingProfile()
       {
           CreateMap<SourceClass, DestinationClass>();
       }
   }
   ```

   

3. 配置 AutoMpper

   在应用程序启动（例如 Startup.cs）中，配置 AutoMapper 以使用映射配置文件

   ```c#
   services.AddAutoMapper(typeof(Startup));
   ```

   

4. 执行映射

   使用 AutoMapper 提供的 Mapper 实例在代码中执行映射

   ```c#
   var destination = _mapper.Map<Destination>(source);
   ```

   

   使用指定的映射配置文件创建 MapperConfiguration 实例

   ```c#
   // 创建映射器配置
   var config = new MapperConfiguration(cfg =>
   {
       cfg.AddProfile<MappingProfile>();
   });
   
   
   // 构建 IMapper 实例
   _mapper = config.CreateMapper();
   ```

   

> 相关 API

1. **CreateMap<TSource, TDestination>() - 配置映射规则**

   用于定义源类型（ TSource ）和目标类型（ TDestination ）之间的映射规则

   ```c#
   // 源类和目标类
   public class Source
   {
       public string Name { get; set; }
   }
   
   public class Destination
   {
       public string FullName { get; set; }
   }
   
   // 映射配置
   public class MappingProfile : Profile
   {
       public MappingProfile()
       {
           CreateMap<Source, Destination>(); // 定义从源到目标的映射
       }
   }
   ```

   

2. **ForMember - 指定特定属性的映射规则**

   用于在映射期间指定属性的来源

   ```c#
   public class MappingProfile : Profile
   {
       public MappingProfile()
       {
           CreateMap<Source, Destination>()
               .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => src.Name)); // Map 'Name' to 'FullName'
       }
   }
   ```

   

3.  **MapFrom - 指定属性的映射源**

   用于在映射期间指定属性的来源

   ```c#
   public class MappingProfile : Profile
   {
       public MappingProfile()
       {
           CreateMap<Source, Destination>()
               .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => src.Name)); // Map 'Name' to 'FullName'
               
           CreateMap<Source, Destination>()
               .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => $"{src.FirstName} {src.LastName}")); // Map 'FirstName' and 'LastName' to 'FullName'
       }
   }
   ```



> 直接注册和使用模块化方式注册

1. **直接注册 AutoMapper **

   ```c#
   public void ConfigureServices(IServiceCollection services)
   {
       services.AddAutoMapper(cfg =>
       {
           cfg.CreateMap<Source, Destination>();
           // 根据实际需要，添加更多映射配置
       });
   }
   ```

   

2. **模块化方式注册 AutoMapper **

   自定义模块示例：

   ```c#
   public class AutoMapperModule : Module
   {
       protected override void Load(ContainerBuilder builder)
       {
           builder.Register(ctx => new MapperConfiguration(cfg =>
           {
               cfg.CreateMap<Source, Destination>();
               // 根据实际需要，添加更多映射配置
           }))
           .AsSelf()
           .SingleInstance();
   
           builder.Register(ctx => ctx.Resolve<MapperConfiguration>().CreateMapper())
               .As<IMapper>()
               .InstancePerLifetimeScope();
       }
   }
   ```

   

   在 Startup 中使用模块注册：

   ```
   public void ConfigureServices(IServiceCollection services)
   {
       // 注册 AutoMapper 模块
       services.AddAutoMapperModule();
   }
   ```

   

   Autofac 注册模块示例

   ```c#
   public void ConfigureContainer(ContainerBuilder builder)
   {
       builder.RegisterModule(new AutoMapperModule());
   }
   ```

