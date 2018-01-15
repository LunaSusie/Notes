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
* modularity（模块化）：为构建可重用模块提供了强大的基础架构。
* Data Filters（数据过滤）：提供自动数据过滤来实现一些模式，如软删除和多租户。
* Multi Tenancy（多租户）：它完全支持多租户，包括单租户架构中的单个数据库或数据库。
* Setting ManageMent（设置管理）：提供强大的基础设施来获取/更改应用程序，租户和用户级别设置。
* Unit & Integration Testing（单元和集成测试）：考虑到可测试性。还提供了基类来简化单元和集成测试。

# 依赖注入
## 传统依赖注入
假设有一个`application service`使用一个`repository`去插入一个`entities`到数据库：
```csharp?linenums
public class PersonAppService
{
	private IPersonRepository _personRepository;
	public PersonAppService()
	{
		_personRepository=new PersonRepository();
	}
	public void CreatePerson(string name ,int age)
	{
		var person=new Person{Name=name, Age=age};
		_personRepository.Insert(person);
	}
}
```
* 组件应该依赖接口而不是实现。PersonAppService中的CreatePerson方法依赖于IPersonRepository和PersonRepository的构造函数。
使用工厂模式：
```csharp?linenums
public class PersonAppService
{
	private IPersonRepository _personRepository;
	public PersonAppService()
	{
		_personRepository=PersonRepositoryFactory.Create();    
	}
	public void CreatePerson(string name ,int age)
	{
		var person=new Person{Name=name, Age=age};
		_personRepository.Insert(person);
	}
}
```
* PersonRepositoryFactory是一个创建并返回一个IPersonRepository的静态类。这被称为服务定位器模式。
* PersonAppService依赖于PersonRepositoryFactory。这是更可以接受的，但仍然有一个硬性依赖。
构造函数注入模式：
```csharp?linenums
public class PersonAppService
{
	private IPersonRepository _personRepository;
	public PersonAppService(IPersonRepository personRepository)
	{
		_personRepository=personRepository;   
	}
	public void CreatePerson(string name ,int age)
	{
		var person=new Person{Name=name, Age=age};
		_personRepository.Insert(person);
	}
}
```
* PersonAppService不知道哪些类实现了IPersonRepository以及如何创建它。谁需要使用PersonAppService，首先创建一个IPersonRepository并将其传递给PersonAppService的构造函数：
```csharp?linenums
var repository=new PresonRepository();
var personService=new PersonAppService(repository);
personService.CreatePerson(''Yuns emre",19);
```
* 依赖类可能有其他依赖关系（这里，PersonRepository可能有依赖关系）。
* 构造器注入模式是提供类的依赖关系的完美方式。通过这种方式，您不能在不提供依赖关系的情况下创建类的实例。
属性注入模式：
```csharp?linenums
public class PersonAppService
{
    public ILogger Logger { get; set; }

    private IPersonRepository _personRepository;

    public PersonAppService(IPersonRepository personRepository)
    {
        _personRepository = personRepository;
        Logger = NullLogger.Instance;
    }

    public void CreatePerson(string name, int age)
    {
        Logger.Debug("Inserting a new person to database with name = " + name);
        var person = new Person { Name = name, Age = age };
        _personRepository.Insert(person);
        Logger.Debug("Successfully inserted!");
    }
}
```
* NullLogger.Instance是实现ILogger的单例对象，但实际上什么都不做（不写日志，它用空方法体实现ILogger）。
* 如果在创建PersonAppService对象之后设置Logger，PersonAppService可以写入日志：
```csharp?linenums
var personService = new PersonAppService(new PersonRepository());
personService.Logger = new Log4NetLogger();
personService.CreatePerson("Yunus Emre", 19);
```
* 假设Log4NetLogger使用Log4Net库实现ILogger并写入日志。
* 因此，PersonAppService实际上可以写日志。如果我们不设置记录器，它不会写入日志。
* ILogger是PersonAppService 的可选依赖。
## 依赖注入框架：Castle Windsor 框架
在一个依赖注入框架中，你首先注册你的接口/类到依赖注入框架，然后你可以解析（创建）一个对象。
```csharp?linenums
var container = new WindsorContainer();

container.Register(
        Component.For<IPersonRepository>().ImplementedBy<PersonRepository>().LifestyleTransient(),
        Component.For<IPersonAppService>().ImplementedBy<PersonAppService>().LifestyleTransient()
    );

var personService = container.Resolve<IPersonAppService>();
personService.CreatePerson("Yunus Emre", 19);
```
* 创建了WindsorContainer。然后 用它们的接口注册 PersonRepository和PersonAppService。
* 然后我们要求容器创建一个IPersonAppService。
## abp依赖注入基础结构
### 注册依赖关系
#### 常规（约定）注册类
ASP.NET Boilerplate按照`约定`自动注册所有的 Repositories，Domain，Application Service，MVC Controllers和Web API Controllers。
例如，您可能有一个IPersonAppService接口和一个实现它的PersonAppService类：
```csharp?linenums
public interface IPersonAppService : IApplicationService
{
    //...
}

public class PersonAppService : IPersonAppService
{
    //...
}
```
* ASP.NET Boilerplate自动注册它，因为它实现了 IApplicationService接口（它只是一个空的接口）。它被注册为瞬态（每个用户创建的实例）。当你往IPersonAppService接口注入（使用构造函数注入）一个类时，PersonAppService对象被创建并自动传入构造函数。
* 命名约定：该例子中的约定是PersonAppService后缀。
* ASP.NET Boilerplate可以按照惯例注册程序集：
```csharp?linenums
IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
```
* Assembly.GetExecutingAssembly（）获取包含此代码的程序集的引用。您可以将其他程序集传递给RegisterAssemblyByConvention方法。
* 您可以通过实现IConventionalRegisterer接口实现自己的常规（约定）注册类，调用 IocManager.AddConventionalRegisterer方法来编写自己的常规（约定）注册 类。你可以添加它在你的模块的预初始化方法。