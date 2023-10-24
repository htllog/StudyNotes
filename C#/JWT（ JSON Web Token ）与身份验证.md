# JWT（ JSON Web Token ）与身份验证

## JWT 结构

JSON Web 令牌 `xxxxx.yyyyy.zzzzz` 用 `.` 分隔三个部分组成，分别是：

* 标头：包含令牌的类型（ JWT ）和所使用的签名算法

  ```
  {
    "alg": "HS256",
    "typ": "JWT"
  }
  ```

  对该 JSON 进行 **Base64Url** 编码加密（加密是可以对称解密）形成  JWT 的第一部分

  ```
  eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
  ```

* 有效载荷：存放有效信息的地方，有效信息包含（注册声明、公共声明、私人声明）

* 签名：获取编码的标头、编码的有效负载、秘密、标头中指定的算法，然后对其进行签名，用于验证消息的完整性和真实性

![image](https://github.com/htllog/StudyNotes/assets/118370026/7f32e6ba-dba9-4fcb-9104-124ee8b696e2)



## JWT Token

### 认证流程

一般是在请求头里加入 `Authorization` ，并加上 `Bearer` 标注：

```bash
fetch('api/user/1', {
  headers: {
    'Authorization': 'Bearer ' + token
  }
})
```

服务端会验证 Token ，如果验证通过就会返回相应的资源，整个流程如下：

![image](https://github.com/htllog/StudyNotes/assets/118370026/60e04e6f-6f85-438d-ac89-48f2c85df1e5)

流程说明：

1. 浏览器发起请求登陆，携带用户名和密码
2. 服务端验证身份，根据算法，将用户标识符打包生成 token
3. 服务器返回 JWT 信息给浏览器，JWT 不包含敏感信息
4. 浏览器发起请求获取用户资料，把刚刚拿到的 token 一起发送给服务器
5.  服务器发现数据中有 token，验明正身
6. 服务器返回该用户的用户资料



### 基于 token 的鉴权机制

- 用户使用用户名密码来请求服务器
- 服务器进行验证用户的信息
- 服务器通过验证发送给用户一个 token 
- 客户端存储 token ，并在每次请求时附送上这个 token 值
- 服务端验证 token 值，并返回数据



## 身份验证方案 AuthorizationType Beare

### 什么是 Beare

Bearer Token ( [RFC 6750  (opens new window)](http://www.rfcreader.com/#rfc6750) ) 用于授权访问资源，任何 Bearer 持有者都可以无差别地用它来访问相关的资源，而无需证明持有加密 key。一个 Bearer 代表授权范围、有效期，以及其他授权事项；一个 Bearer 在存储和传输过程中应当防止泄露，需实现 Transport Layer Security  ( TLS )；一个 Bearer 有效期不能过长，过期后可用 Refresh Token 申请更新

建议开发者遵循规范，在每次请求的 Token 前附带 Bearer



### 使用 Bearer Token 身份验证

1. **API 访问控制**

   当你构建一个需要授权用户访问的 API 时，你通常会使用 Bearer Token 身份验证。客户端在登录后会收到一个 Bearer Token，它需要将令牌包含在每个请求的头部，以便访问受保护的资源

2. **OAuth 2.0**

   OAuth 2.0 是一个授权协议，它允许第三方应用程序获得用户授权并访问受保护资源。Bearer Token 常用于 OAuth 2.0 身份验证，用于传递访问令牌，以验证第三方应用程序的访问权限

3. **JWT ( JSON Web Tokens )**

   JWT 是一种用于创建和验证 Bearer Token 的标准。JWT 令牌通常被用于安全地传递用户身份和声明信息

**Tips : **并非所有的身份验证方案都使用 Bearer Token 。有些身份验证方案可能需要用户名和密码 ( token )，而不是 Bearer Token。这些方案通常使用 Basic Authentication，其中用户名和密码以 Base64 编码的形式包含在请求头部



### 身份验证方案 AuthorizationType

在身份验证方案中，" AuthorizationType Bearer " 通常是指使用 Bearer Token 进行身份验证。这是一种常见的身份验证方式，用于验证 API 请求的有效性。Bearer Token 是一种包含在 HTTP 请求头部的令牌，通常采用以下格式：

```makefile
Authorization: Bearer YOUR_TOKEN
```

* " YOUR_TOKEN " 代表你的身份验证令牌

* Bearer Token 被用于访问受保护的资源，如 API 端点。当客户端发送 API 请求时，它需要将 Bearer Token 包含在请求头中以证明其合法性。API 服务器将验证令牌的有效性，如果验证成功，请求将被处理，否则，请求将被拒绝

使用 Bearer Token 进行身份验证通常用于 API 访问控制、OAuth 2.0 授权和  JWT 身份验证，而用户名和密码 ( token ) 可能用于其他类型的身份验证



### Authorization:  YOUR_TOKEN

如果接口请求头参数使用的是 "Authorization:  YOUR_TOKEN"，不是标准的 " Authorization: Bearer YOUR_TOKEN "，意味着正在使用自定义的身份验证头部，而不是标准的 Bearer Token 头部

使用自定义的身份验证头部需要确保你的服务器端代码能够正确地解析和验证这种自定义头部的令牌。在服务器端代码中，你需要提取 " YOUR_TOKEN " 的值，并使用适当的身份验证逻辑来验证令牌的有效性



## 配置 JWT（ JSON Web Token ）身份验证



## 配置过期时间

可以在应用程序的配置中定义这个有效期，例如在  `appsettings.json`  文件中

```json
{
  "Authentication": {
    "SecretKey": "your_secret_key",
    "ExpiresIn": "00:30:00" // 30 minutes
  }
}
```



然后，你可以在配置中读取这个有效期，将其解析为 `TimeSpan`，并将其用于设置  JWT 令牌的过期时间

```c#
string expiresIn = configuration.GetValue<string>("Authentication:ExpiresIn");
if (TimeSpan.TryParse(expiresIn, out TimeSpan expirationTime))
{
    var tokenDescriptor = new SecurityTokenDescriptor
    {
        // ... 其他配置 ...

        Expires = DateTime.UtcNow.Add(expirationTime), // 设置过期时间
        // ... 其他配置 ...
    };
}
```



## 关于前后端的登录验证

### 前端验证

前端的验证只是表面上的验证，用来提供用户友好的反馈。例如，前端可以在用户未提供用户名和密码或没有相关权限时显示错误消息，但前端验证可以被绕过。如果前端验证失败，用户仍然可以尝试手动访问受保护的资源，而这个时候，后端的验证就起作用了

### 后端验证

后端的验证是应用程序的最后一道防线，它不仅仅依赖于用户提供的信息，还依赖于后端系统的内部状态和数据。通过后端验证，应用程序可以确保：

1. 用户提交的信息是有效的：前端验证只是确保用户填写了必要字段，而后端验证可以检查这些字段的内容是否符合期望，比如密码是否正确
2. 用户的权限得到有效的检查：前端验证只能提供基本的权限检查，但它不能替代后端验证，因为用户可以篡改前端验证或直接访问后端资源。只有后端验证能够确保用户具有访问受保护资源的权限
3. 数据完整性得到维护：后端验证可以确保从客户端传递的数据符合应用程序的期望，并且不会对数据库或其他资源造成损害

总之，前端验证和后端验证各有其重要性。前端验证提供了用户友好的反馈和提前检查，但不应信任前端验证，因为它可以被绕过。后端验证是应用程序的最后一道防线，确保应用程序的安全性和完整性。因此，安全的应用程序应该同时使用前端验证和后端验证

## 参考

[ JWT（ JSON Web Token ）](https://github.com/OhlinC/StudyNotes/blob/main/JWT（JSON Web Token）.md#jwtjson-web-token)

[JWT](https://jwt.io/introduction)

[在线 JWT Token 解析解码](https://tooltt.com/jwt-decode/)

