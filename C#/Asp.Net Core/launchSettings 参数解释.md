```markdown
{
  // list 配置
  "iisSettings": {
    "windowsAuthentication": false,  // 是否启用 Windows 身份验证
    "anonymousAuthentication": true, // 指定是否启用匿名身份验证
    "iisExpress": {
      "applicationUrl": "http://localhost:2301", // Express 应用程序的基本 URL
      "sslPort": 44320 // SSL 端口号
    }
  },
  // 为应用程序定义不同的启动配置文件
  "profiles": {
    // http：HTTP 的启动配置文件：
    "http": {
      "commandName": "Project", // 指定要执行的命令（例如，Project 表示使用 dotnet run）
      "dotnetRunMessages": true, // 确定是否显示来自 dotnet run 的消息
      "launchBrowser": true, // 指定是否启动默认浏览器
      "applicationUrl": "http://localhost:5201", // 应用程序的基本 URL
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development" // 环境变量：应用程序的附加环境变量
      }
    },
    // https：HTTPS 的启动配置文件
    // 与 http 配置文件类似的参数，但适用于 HTTPS
    "https": {
      "commandName": "Project",  
      "dotnetRunMessages": true,
      "launchBrowser": true,
      // 允许指定多个 applicationUrl 值（例如，对于不同的端口）
      "applicationUrl": "https://localhost:7019;http://localhost:5201",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    // IIS Express 的启动配置文件
    "IIS Express": {
      "commandName": "IISExpress", // 指定要执行的命令 (IISExpress)
      "launchBrowser": true, // 指定是否启动默认浏览器
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development" // 应用程序的附加环境变量
      }
    }
  }
}
```

