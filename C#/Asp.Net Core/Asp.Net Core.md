# Asp.Net Core 文件结构

1. **Properties**

   **launchSettings.json**: 配置应用程序的启动设置，包括启动 URL 、环境变量等

2. **wwwroot**

   存放静态资源，如图像、CSS 文件、JavaScript 文件等，可以直接通过浏览器访问

3. **Controllers**

   存放控制器类，控制器负责处理HTTP请求并返回相应的结果

4. **Models**:

   存放应用程序的模型类，用于表示应用程序的数据结构

5. **Views**

   存放视图文件，通常使用 Razor 视图引擎，用于生成HTML响应

6. **Areas** (可选)

   包含按功能或业务领域划分的子区域，每个子区域可以有独立的 Controllers 、Views 等

7. **Data** (可选)

   存放数据访问层的代码，如 DbContext 类、数据实体等

8. **Services** (可选)

   存放应用程序的服务类，用于提供特定功能或业务逻辑

9. **Extensions** (可选)

   存放自定义扩展方法或类

10. **Middleware** (可选)

    存放自定义中间件

11. **Utilities** (可选)

    存放实用工具类

12. **ViewModels** (可选)

    存放视图模型，用于将数据从控制器传递到视图

13. **AppSettings.json**

    应用程序的配置文件，包含应用程序的配置信息，如数据库连接字符串等

14. **Startup.cs**

    包含应用程序的启动配置，配置服务、中间件等

15. **Program.cs**

    应用程序的入口点，负责创建 WebHost 并启动应用程序

16. **.csproj文件**

    项目文件，定义项目的依赖项和构建配置

17. **Startup.cs**

    配置应用程序的启动和初始化过程，包括服务注册、中间件配置等

    

```mathematica
ProjectRoot
│
├── Properties
│   └── launchSettings.json
│
├── wwwroot
│   ├── images
│   ├── css
│   └── js
│
├── Controllers
│   ├── HomeController.cs
│   └── ...
│
├── Models
│   ├── User.cs
│   └── ...
│
├── Views
│   ├── Home
│   │   ├── Index.cshtml
│   │   └── ...
│   └── ...
│
├── Areas
│   ├── Admin
│   │   ├── Controllers
│   │   │   ├── AdminController.cs
│   │   │   └── ...
│   │   ├── Views
│   │   │   ├── Index.cshtml
│   │   │   └── ...
│   │   └── ...
│   └── ...
│
├── Data
│   ├── ApplicationDbContext.cs
│   └── ...
│
├── Services
│   ├── UserService.cs
│   └── ...
│
├── Extensions
│   ├── StringExtensions.cs
│   └── ...
│
├── Middleware
│   ├── LoggingMiddleware.cs
│   └── ...
│
├── Utilities
│   ├── FileHelper.cs
│   └── ...
│
├── ViewModels
│   ├── HomeViewModel.cs
│   └── ...
│
├── appsettings.json
│
├── Startup.cs
│
├── Program.cs
│
└── YourProjectName.csproj

```

