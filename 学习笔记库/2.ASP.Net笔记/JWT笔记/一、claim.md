## 一、什么是claim?

嗯，这么长个例子，其实和Claim没什么关系 

门票上有什么？我们来假设一下

门票上有，姓名，票价，性别，地址，【等等身份证一类的东东】

好了，我们假设的门票就这样，从门票的第二行（姓名...）开始,每一行都是一个Claim

有了上面的铺垫，我们接下来正式介绍下Claim:

在身份验证和授权领域，"Claim"（声明）是关于主体（用户、设备或实体）的一些属性或特征的声明性陈述。它是一种用于在安全令牌（如 JSON Web Token）中传递有关用户身份和权限的信息的数据结构。

声明通常包含有关主体的信息，例如姓名、电子邮件地址、角色、权限等。它们用于验证用户的身份和授权用户对资源的访问。

在.NET Framework 和 .NET Core 中，`System.Security.Claims` 命名空间提供了用于处理声明的类和接口。`Claim` 类表示一个声明，它包含了声明的类型（名称）和值。例如，`new Claim("name", "John")` 表示一个名称为 "name"，值为 "John" 的声明。

声明可以用于各种身份验证和授权场景，例如基于角色的访问控制、声明式身份验证和单点登录（SSO）等。

## 二、怎么使用claim？

使用 `new Claim(type, value)` 构造函数创建一个声明对象，其中 `type` 是声明的类型（名称），`value` 是声明的值。

例如:

```
//var claims = new Claim[] //old
var claims = new List<Claim>
    {
        new Claim(JwtRegisteredClaimNames.Jti, tokenModel.Uid.ToString()),
        new Claim(JwtRegisteredClaimNames.Iat, $"{DateTime.Now.DateToTimeStamp()}"),
        new Claim(JwtRegisteredClaimNames.Nbf,$"{DateTime.Now.DateToTimeStamp()}") ,
        new Claim (JwtRegisteredClaimNames.Exp,$"{new DateTimeOffset(DateTime.Now.AddSeconds(1000)).ToUnixTimeSeconds()}"),
        new Claim(ClaimTypes.Expiration, DateTime.Now.AddSeconds(1000).ToString()),
        new Claim(JwtRegisteredClaimNames.Iss,iss),
        new Claim(JwtRegisteredClaimNames.Aud,aud),
    	new Claim(ClaimTypes.Role,tokenModel.Role)
   };
claims.AddRange(tokenModel.Role.Split(',').Select(s => new Claim(ClaimTypes.Role, s)));
```

### 结构体 `JwtRegisteredClaimNames`

其中包含了一些已注册的声明（claims）的常量字段。

JWT（JSON Web Token）是一种用于在网络应用之间传递声明（claims）的安全方式。声明是关于实体（通常是用户）和其他数据的声明性语句。这些常量字段表示了一些常见的声明名称，这些名称在不同的标准中被定义和使用。

以下是一些常量字段的解释：

- `Actort`：表示代理人（actor），即执行操作的用户或系统。
- `Acr`：表示身份验证上下文参考（Authentication Context Class Reference），用于指示用户进行身份验证时使用的身份验证上下文。
- `Amr`：表示认证方法（Authentication Methods），用于指示用户进行身份验证时使用的认证方法。
- `Aud`：表示接收方（Audience），即 JWT 的预期接收方。
- `AuthTime`：表示身份验证时间，即用户进行身份验证的时间。
- `Azp`：表示授权方（Authorized Party），即授予访问令牌的授权服务器。
- `Birthdate`：表示出生日期。
- `CHash`：表示授权代码哈希（Authorization Code Hash），用于验证授权代码的完整性。
- `AtHash`：表示访问令牌哈希（Access Token Hash），用于验证访问令牌的完整性。
- `Email`：表示电子邮件地址。
- `Exp`：表示过期时间，即 JWT 的过期时间。
- `Gender`：表示性别。
- `FamilyName`：表示姓氏。
- `GivenName`：表示名字。
- `Iat`：表示发布时间，即 JWT 的发布时间。
- `Iss`：表示发布方（Issuer），即 JWT 的发布方。
- `Jti`：表示 JWT 的唯一标识符。
- `Name`：表示名称。
- `NameId`：表示名称标识符。
- `Nonce`：表示随机数，用于防止重放攻击。
- `Nbf`：表示生效时间，即 JWT 的生效时间。
- `Prn`：表示代理人（Principal），即代表用户进行操作的实体。
- `Sid`：表示会话标识符（Session ID），用于标识用户的会话。
- `Sub`：表示主题（Subject），即 JWT 所描述的实体。
- `Typ`：表示类型（Type），用于指示 JWT 的类型。
- `UniqueName`：表示唯一名称。
- `Website`：表示网站。

### `ClaimTypes` 

是 .NET Framework 和 .NET Core 中的一个类，定义了一组常见的声明类型（名称）作为静态字段。这些常见的声明类型包括姓名、电子邮件地址、角色、权限等。例如，`ClaimTypes.Name` 表示姓名声明，`ClaimTypes.Email` 表示电子邮件地址声明。

- `Actor`: 表示声明中指定的操作者的 URI。
- `PostalCode`: 表示实体的邮政编码的 URI。
- `PrimaryGroupSid`: 表示实体的主要组安全标识符（SID）的 URI。
- `PrimarySid`: 表示实体的主要安全标识符（SID）的 URI。
- `Role`: 表示实体的角色的 URI。
- `Rsa`: 表示一个 RSA 密钥的 URI。
- `SerialNumber`: 表示实体的序列号的 URI。
- `Sid`: 表示一个安全标识符（SID）的 URI。
- `Spn`: 表示一个服务主体名称（SPN）声明的 URI。
- `StateOrProvince`: 表示实体所属的州或省份的 URI。
- `StreetAddress`: 表示实体的街道地址的 URI。
- `Surname`: 表示实体的姓氏的 URI。
- `System`: 表示系统声明的 URI。
- `Thumbprint`: 表示实体的指纹的 URI。
- `Upn`: 表示实体的用户主体名称（UPN）的 URI。
- `Uri`: 表示一个 URI 的声明的 URI。
- `UserData`: 表示实体的用户数据的 URI。
- `Version`: 表示声明的版本的 URI。
- `Webpage`: 表示实体的网页的 URI。
- `WindowsAccountName`: 表示 Windows 帐户名称的 URI。
- `WindowsDeviceClaim`: 表示 Windows 设备声明的 URI。
- `WindowsDeviceGroup`: 表示 Windows 设备组的 URI。
- `WindowsFqbnVersion`: 表示 Windows 完全限定域名（FQBN）版本的 URI。
- `WindowsSubAuthority`: 表示 Windows 子权限的 URI。
- `OtherPhone`: 表示实体的其他电话号码的 URI。
- `NameIdentifier`: 表示实体的名称标识符的 URI。
- `Name`: 表示实体的名称的 URI。
- `MobilePhone`: 表示实体的移动电话号码的 URI。
- `Anonymous`: 表示匿名声明的 URI。
- `Authentication`: 表示身份验证声明的 URI。
- `AuthenticationInstant`: 表示身份验证的时间戳的 URI。
- `AuthenticationMethod`: 表示身份验证方法的 URI。
- `AuthorizationDecision`: 表示授权决策的 URI。
- `CookiePath`: 表示 Cookie 路径的 URI。
- `Country`: 表示实体所属的国家的 URI。
- `DateOfBirth`: 表示实体的出生日期的 URI。
- `DenyOnlyPrimaryGroupSid`: 表示仅拒绝的主要组安全标识符（SID）的 URI。
- `DenyOnlyPrimarySid`: 表示仅拒绝的主要安全标识符（SID）的 URI。
- `DenyOnlySid`: 表示仅拒绝的安全标识符（SID）的 URI。
- `WindowsUserClaim`: 表示 Windows 用户声明的 URI。
- `DenyOnlyWindowsDeviceGroup`: 表示仅拒绝的 Windows 设备组的 URI。
- `Dsa`: 表示一个 DSA 密钥的 URI。
- `Email`: 表示实体的电子邮件地址的 URI。
- `Expiration`: 表示声明的过期时间的 URI。
- `Expired`: 表示声明是否已过期的 URI。
- `Gender`: 表示实体的性别的 URI。
- `GivenName`: 表示实体的名字的 URI。
- `GroupSid`: 表示组安全标识符（SID）的 URI。
- `Hash`: 表示实体的哈希值的 URI。
- `HomePhone`: 表示实体的家庭电话号码的 URI。
- `IsPersistent`: 表示声明是否是持久性的 URI。
- `Locality`: 表示实体所在地的 URI。
- `Dns`: 表示实体的 DNS 名称的 URI。
- `X500DistinguishedName`: 表示 X.500 区分名称的 URI。