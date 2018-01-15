---
title: abp学习
tags: abp
grammar_cjkRuby: true
---

# 介绍
## 主要功能
* Dependency Injection （依赖注入）：ABP使用并提供强大而传统的DI基础设施。
* Repository （仓储）：ABP可以为每个实体创建一个默认存储库。存储库抽象DBMS和ORM并简化数据访问逻辑。
* Authorization（授权）：ABP可以检查权限。它使用声明性属性简化授权，但也有其他授权方式。
* Validation（验证）：ABP会自动检查输入是否为空。它还基于标准数据注释属性和自定义验证规则验证输入的所有属性。
* Audit Logging（审计日志）：根据约定和配置，为每个请求自动保存用户，浏览器，IP地址，呼叫服务，方法，参数，呼叫时间，执行时间等信息。
* Unit Of Work（工作单元）：在ABP中，每个应用程序服务方法被假设为默认的工作单元。
* Exception Handing（异常处理）：在Web应用程序中，我们几乎从不处理ABP中的异常。所有的异常都会默认自动处理。如果发生异常，ABP将自动记录并向客户端返回正确的结果。
* Loggin（日志）：我们可以使用基类中定义的记录器对象来编写日志。Log4Net用作默认值，但是可变或可配置。
* Localization（本地化）：它会根据当前用户的文化自动进行本地化。
* Auto Mapping （自动映射）：使用AutoMapper库来执行映射。因此，根据命名约定，我们可以轻松地将属性从一个对象映射到另一个对象。
* Dynamic web api layer（动态webapi）：我们通常编写一个封装Web API控制器来向JavaScript客户端公开方法。ABP在运行时自动执行该操作。
* Dynamic Javascript AJAX Proxy（动态javascriptAjax代理）：ABP创建JavaScript代理方法，这些方法调用应用程序服务方法就像调用客户端上的JavaScript方法一样简单。
## 其他功能
* modularity（模块化）：
* Data Filters（数据过滤）：
* Multi Tenancy（多租户）：
* Setting ManageMent（）：
* Unit & Integration Testing
