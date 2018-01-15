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
	* Description：一个可本地化的字符串，可以用来在UI中显示权限的定义。
	* MultiTenancySides：对于多租户应用程序，租户或主机可以使用许可。这是一个标志枚举，因此双方都可以使用权限。
	* featureDependency：可用于声明对特征的依赖关系 。因此，只有在满足特征依赖关系的情况下才能授予此权限。它等待一个实现IFeatureDependency的对象。默认实现是SimpleFeatureDependency类。用法示例： new SimpleFeatureDependency("MyFeatureName")
* 权限可以具有父权和子权限。虽然这不会影响权限检查，但可能有助于在UI中对权限进行分组。
* 创建授权提供者后，我们应该在我们模块的PreInitialize方法中注册它：
```csharp?linenums
Configuration.Authorization.Providers.Add<MyAuthorizationProvider>();
```
* 授权提供程序自动注册到依赖注入。因此，授权提供者可以注入任何依赖项（如存储库）以使用其他来源构建权限定义。
## 检查权限
### 使用AbpAuthorize特性
AbpAuthorize（AbpMvcAuthorize为MVC控制器和 AbpApiAuthorize的Web API控制器）属性是检查权限的最简单，最常用的方法。
```csharp?linenums
[AbpAuthorize("Administration.UserManagement.CreateUser")]
public void CreateUser(CreateUserInput input)
{
    //A user can not execute this method if he is not granted for "Administration.UserManagement.CreateUser" permission.
}
```
* CreateUser方法只能被授予“ Administration.UserManagement.CreateUser ” 权限的用户调用。
* AbpAuthorize属性还检查当前用户是否登录
```csharp?linenums
[AbpAuthorize]
public void SomeMethod(SomeMethodInput input)
{
    //A user can not execute this method if he did not login.
}
```
### AbpAuthorize特性限制
不能用于私人方法。
不能用于静态方法。
不能用于非注入类的方法
如果通过接口调用方法（如通过接口使用的应用程序服务），可以将其用于任何公共方法 。
如果直接从类引用（如ASP.NET MVC或Web API控制器）中调用方法，该方法应该是虚拟的。
如果受保护，方法应该是虚拟的。
### 有四种类型的授权特性
在应用程序服务（应用程序层）中，我们使用 Abp.Authorization.AbpAuthorize属性。
在MVC控制器（web层）中，我们使用 Abp.Web.Mvc.Authorization.AbpMvcAuthorize属性。
在`ASP.NET Web API`中，我们使用 Abp.WebApi.Authorization.AbpApiAuthorize属性。
在`ASP.NET Core`中，我们使用 Abp.AspNetCore.Mvc.Authorization.AbpMvcAuthorize属性。
### 禁止授权
您可以通过将AbpAllowAnonymous属性添加到应用程序服务来禁用方法/类的授权 。
对于`MVC`，`Web API`和`ASP.NET Core Controllers` 使用 `AllowAnonymous`，它们是这些框架的本地属性。
### 使用IPermissionChecker
虽然AbpAuthorize属性对于大多数情况来说已经足够了，但是我们必须检查方法体中的权限。
我们可以注入并使用IPermissionChecker：
```csharp?linenums
public void CreateUser(CreateOrUpdateUserInput input)
{
    if (!PermissionChecker.IsGranted("Administration.UserManagement.CreateUser"))
    {
        throw new AbpAuthorizationException("You are not authorized to create user!");
    }

    //A user can not reach this point if he is not granted for "Administration.UserManagement.CreateUser" permission.
}
```
如果您只是检查权限并抛出异常，您可以使用Authorize 方法：
```csharp?linenums
public void CreateUser(CreateOrUpdateUserInput input)
{
    PermissionChecker.Authorize("Administration.UserManagement.CreateUser");

    //A user can not reach this point if he is not granted for "Administration.UserManagement.CreateUser" permission.
}
```
### Razor Views
基本视图类定义了IsGranted方法来检查当前用户是否有权限。因此，我们可以有条件地渲染视图。
```csharp?linenums
@if (IsGranted("Administration.UserManagement.CreateUser"))
{
    <button id="CreateNewUserButton" class="btn btn-primary"><i class="fa fa-plus"></i> @L("CreateNewUser")</button>
}
```
### 客户端（Javascript）
在客户端，我们可以使用在abp.auth命名空间中定义的API 。在大多数情况下，我们需要检查当前用户是否具有特定权限（使用权限名称）。
```csharp?linenums
abp.auth.isGranted('Administration.UserManagement.CreateUser');
```
您还可以使用abp.auth.grantedPermissions让所有授予的权限或abp.auth.allPermissions获得应用程序中所有可用的权限名。在其他运行时检查abp.auth命名空间。