> 引入相关依赖包

```bash
install package Hangfire.Core
install package Hangfire.AspNetCore
install package Hangfire.MySqlStorage
install-Package Microsoft.Extensions.Configuration.Bind
```



>Stratup 文件中配置

```c#
app.UseHangfireDashboard(); // 配置后台仪表盘
app.UseEndpoints(endpoints =>
{
  	endpoints.MapControllers();
		// 将 Hangfire 仪表板添加到应用程序的路由中
    endpoints.MapHangfireDashboard();
});
```



> 在 appsettings.json 中配置连接字符串

```json
{
  "ConnectionStrings": {
    "HangfireConnection": "需要连接的字符串"
  },
  // ...
}
```



> 创建 Hangfire  配置类，通过拓展用于注册服务集合





> 相关参数

1. `QueuePollInterval`

   设置 Hangfire 轮询作业队列的时间间隔。这影响 Hangfire 如何检查待处理的作业

2. `JobExpirationCheckInterval`

   检查作业过期的时间间隔。Hangfire 将使用此参数来检查是否有已过期的作业需要清理

3. `CountersAggregateInterval`

   设置计数器信息的聚合时间间隔。影响 Hangfire 如何聚合计数器信息

4. `PrepareSchemaIfNecessary`

   如果设置为 `true`，Hangfire 将在 MySQL 数据库中自动创建所需的数据库架构。这对于第一次设置 Hangfire 很有用

5. `DashboardJobListLimit`

   这是 Hangfire 仪表板中显示的作业列表的限制。可以限制要在仪表板上显示的作业数量，以防止大量作业拥堵仪表板

6. `TransactionTimeout`

   这是数据库事务的超时时间。如果在此时间内事务未能完成，将自动回滚

7. `TablesPrefix`

   这是 Hangfire 数据库中表的前缀。你可以使用此选项来指定表名前缀，以便与其他数据库对象区分开

8. `TransactionIsolationLevel`

   设置数据库事务的隔离级别。默认情况下，它设置为 `IsolationLevel.ReadCommitted`。你可以根据需要更改隔离级别

9. `UseTransactions`

   指示 Hangfire 是否使用数据库事务。默认值为 `true`，表示使用事务

10. `DisableGlobalLocks`

    如果启用全局锁（默认情况下），Hangfire 会锁定所有服务器上的表，以确保在处理作业时不会发生冲突。如果希望禁用全局锁，可以将此参数设置为 `true`

11. `MySqlVariables`

    这是一个字典，允许你为 MySQL 存储设置自定义 MySQL 变量。这些变量可以影响 Hangfire 在 MySQL 数据库上的行为

12. `MySqlDefaultSchema`

    设置默认的 MySQL 架构名称。如果需要在特定架构中创建表，可以使用此参数

13. `AdditionalFilter`

    这个选项允许你为 Hangfire 查询设置额外的过滤条件。这可以用于筛选特定作业的条件

14. `DequeueBatchSize`

    设置 Hangfire 一次从队列中出队多少作业。这可以影响作业的处理方式