>  DbUp 的主要功能和用途

DbUp 是一个开源 .NET 库，可简化数据库部署和迁移的过程。它提供了一个框架来管理和版本控制您的数据库架构和数据脚本，从而更轻松地确保您的数据库在不同环境（例如开发、测试、生产）中始终处于所需状态

1. 数据库版本控制和变更管理

   DbUp 允许您对数据库架构更改进行版本控制，并以结构化和有组织的方式管理它们

2. 数据库部署自动化

   提供了一种简单且自动化的方法来跨各种环境部署数据库更改，而无需手动脚本或步骤

3. 数据库迁移

   DbUp 支持将数据库从当前版本迁移到所需版本，根据需要应用架构更改和数据更新

4. 脚本执行

   DbUp 可以针对目标数据库执行 SQL 脚本，包括架构更改（例如，表创建、列修改）和数据更新

5. 与构建和发布流程集成

   它可以集成到您的构建和发布流程中，确保数据库更新成为持续集成/持续部署 (CI/CD) 管道的一部分

6. 记录和报告

   DbUp 提供日志记录功能，使您能够监控数据库更新过程并报告部署期间可能发生的任何错误或问题



> DbUp 工作原理的简述

DbUp 通常与数据库管理系统（例如 SQL Server、MySQL、PostgreSQL ）结合使用，并且可以在各种 .NET 应用程序（例如 ASP.NET、控制台应用程序）中使用，以确保数据库的一致性

* 您可以为数据库更改定义 SQL 脚本（例如，创建表、修改列）
* DbUp 按特定顺序读取并执行这些脚本来更新目标数据库
* DbUp 在数据库中维护一个版本控制表 SchemaVersions 来跟踪应用的脚本并管理数据库的状态
   ( Tips : 根据这个表判断是否有执行过，避免重复执行)

通过使用 DbUp ，开发人员和数据库管理员可以更有效地管理数据库更新，减少手动错误，并确保数据库结构和内容与应用程序保持同步



> 部署

使用 NuGet 包管理器安装 DbUp 包

```bash
dotnet add package DbUp
```



配置 DbUp 后

```c#
var upgradeEngine = DeployChanges.To
    .SqlDatabase(connectionString)
    .WithScriptsEmbeddedInAssembly(Assembly.GetExecutingAssembly())
    .LogToConsole()
    .Build();
```





* 获取将要执行的脚本 ( `GetScriptsToExecute()`)
* 获取已执行的脚本 ( `GetExecutedScripts()`)
* 检查是否需要升级 ( `IsUpgradeRequired()`)
* 为任何新的迁移脚本创建版本记录而不执行它们 ( `MarkAsExecuted`)
* 对于使开发环境与自动化环境同步很有用
* 尝试连接数据库 ( `TryConnect()`)
* 执行数据库升级 ( `PerformUpgrade()`)
* 日志脚本输出 ( `LogScriptOutput()`)



> 关于 SSL / TLS 协商存在问题

在登录前遇到错误 (" An internal exception was caught ")，通常表示与 SSL/TLS 加密相关的问题

1. **使用 127.0.0.1 而不是 localhost**：尝试在连接字符串中使用`127.0.0.1`而不是。`localhost`有时，使用`localhost`可能会导致与 SSL 证书相关的问题

   ```
   "ConnectionStrings": {
       "DefaultConnectionString": "server=127.0.0.1"
   }
   ```

   

2. **在连接字符串中禁用 SSL**：可以尝试在连接字符串中禁用 SSL 加密，看看是否可以解决问题

   ```c#
   "ConnectionStrings": {
       "DefaultConnectionString": "...Encrypt=false;"
   }
   ```

   

3. **检查 SQL Server Docker 容器设置**

   确保在 SQL Server Docker 容器中正确配置 SSL。SSL 设置可能会根据特定 SQL Server 版本及其在 Docker 容器中的配置方式而有所不同

4. **检查 SQL Server 配置管理器**

   验证 SQL Server 配置管理器的 SSL / TLS 设置。确保协议已启用并正确配置

5. **检查 SQL Server 日志**

   检查 SQL Server 错误日志以获取更详细的错误消息。它可能会为 SSL / TLS 协商问题提供更多见解

   

>关于数据库升级

`DeployChanges.To.SqlDatabase(_connectionString)` 和 `DeployChanges.To.MySqlDatabase(_connectionString)`

1. **`DeployChanges.To.SqlDatabase(_connectionString)`**

   当要对SQL Server数据库进行数据库升级时，可以使用此方法。它指定目标数据库是 SQL Server 数据库。提供`_connectionString`的应该是特定于 SQL Server 的连接字符串

2. **`DeployChanges.To.MySqlDatabase(_connectionString)`**

   当要对 MySQL 数据库进行数据库升级时，可以使用此方法。它指定目标数据库是 MySQL 数据库。提供`_connectionString`的应该是特定于 MySQL 的连接字符串

综上所述，区别在于目标数据库类型

- `SqlDatabase` 对于 SQL Server
- `MySqlDatabase ` 对于 MySQL

根据要执行升级的数据库类型选择适当的方法。提供给每个方法的连接字符串应与数据库类型匹配



> 关于 seq 打印重复日志

Seq 是一个日志服务器和聚合器，用于集中式日志管理和分析。它允许您从各种应用程序和系统收集日志事件，集中存储它们，并分析它们以深入了解应用程序的行为和性能

在 Seq 打印两个日志条目时遇到的问题可能有几个潜在原因：

1. **多个日志源**

   如果您的应用程序或您使用的日志框架配置为将日志事件发送到 Seq ，则应用程序可能会为同一操作生成两个日志事件，从而导致 Seq 中出现重复的日志条目

2. **配置问题**：日志记录框架的配置或应用程序如何设置以将日志发送到 Seq 可能会导致重复的日志条目。检查您的日志记录配置，确保其正确设置为将日志发送到 Seq

3. **日志库行为**：某些日志库可能具有在将日志发送到某些日志服务器（包括 Seq ）时导致重复日志条目的行为或配置

要排查并解决此问题，请考虑以下步骤：

- 检查应用程序的日志记录配置，确保其正确设置为将日志发送到 Seq，并且不会无意中生成重复日志。
- 检查您正在使用的日志框架或库。验证在使用 Seq 作为日志记录端点时是否有任何可能导致重复日志的特定设置或行为。
- 通过将日志发送到不同的目标来测试日志记录行为



> **`LogToAutodetectedLog()`** 和 **`LogToConsole()`**

当同时使用 `LogToAutodetectedLog()` 和 `LogToConsole()` 时，可能会导致将重复的日志条目打印到控制台。为了防止这种情况，您应该根据您的偏好和用例选择两种方法之一

```c#
var upgrade = DeployChanges.To
		.SqlDatabase(_connectionString)
		.WithScriptsEmbeddedInAssembly(Assembly.GetExecutingAssembly())
		.LogToAutodetectedLog()
    .LogToConsole()
		.Build();
```



1. **`LogToAutodetectedLog()`**
   - 说明：此方法是 DbUp 的一部分，用于在数据库升级过程中配置日志记录。它允许 DbUp 根据环境自动检测并使用适当的日志记录机制。例如，它可能会根据可用情况使用控制台、文件或其他日志记录提供程序
   - 何时使用：`LogToAutodetectedLog()` 当希望 DbUp 根据环境选择适当的日志记录机制而不指定特定的日志记录方法时使用
2. **`LogToConsole()`**
   - 说明：此方法将 DbUp 配置为将消息显式记录到控制台
   - 何时使用：`LogToConsole()` 当想要确保日志消息专门定向到控制台时使用，无论环境或其他日志记录机制如何



> 官网

[DbUp](https://dbup.readthedocs.io/en/latest/)