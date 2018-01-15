---
title: abp-授权
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

# 授权
## 介绍
几乎所有的企业应用程序都使用某种级别的授权。授权用于检查是否允许用户在应用程序中执行某些特定的操作。
## 关于IPermissionChecker
授权系统使用IPermissionChecker来检查权限。
虽然你可以用你自己的方式来实现它，但它完全在 模块零项目中实现。
如果没有实现，则使用NullPermissionChecker，将所有权限授予每个人。
## 定义权限
为每个需要授权的操作定义一个唯一的权限。
ASP.NET Boilerplate被设计为模块化的。所以，不同的模块可以有不同的权限。
一个模块应该创建一个派生自AuthorizationProvider的类来定义它的权限。
```csharp?linenums
public class MyAuthorizationProvider : AuthorizationProvider
{
    public override void SetPermissions(IPermissionDefinitionContext context)
    {
        var administration = context.CreatePermission("Administration");

        var userManagement = administration.CreateChildPermission("Administration.UserManagement");
        userManagement.CreateChildPermission("Administration.UserManagement.CreateUser");

        var roleManagement = administration.CreateChildPermission("Administration.RoleManagement");
    }
}
```
* IPermissionDefinitionContext有获取和创建权限的方法。
* 权限属性：
	* name：系统范围的唯一名称。
	* display name：稍后在UI中可用于显示权限的可本地化的字符串。
	* 1
	* MultiTenancySides：对于多租户应用程序，租户或主机可以使用许可。这是一个标志枚举，因此双方都可以使用权限。
	* featureDependency：可用于声明对特征的依赖关系 。因此，只有在满足特征依赖关系的情况下才能授予此权限。它等待一个实现IFeatureDependency的对象。默认实现是SimpleFeatureDependency类。用法示例： new SimpleFeatureDependency("MyFeatureName")