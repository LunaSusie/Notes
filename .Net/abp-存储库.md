---
title: abp-存储库笔记
tags: Repositories
grammar_cjkRuby: true
---

# 存储库
通常，每个实体（或聚合根）使用一个分离的存储库。
## 默认存储库
一个存储库类实现了 IRepository \<TEntity，TPrimaryKey\>接口。
ABP可以为每个实体类型自动创建默认存储库。
您可以直接注入 IRepository \<TEntity\>（或IRepository \<TEntity，TPrimaryKey\>）。
```csharp?linenums
public class PersonAppService : IPersonAppService
{
    private readonly IRepository<Person> _personRepository;

    public PersonAppService(IRepository<Person> personRepository)
    {
        _personRepository = personRepository;
    }

    public void CreatePerson(CreatePersonInput input)
    {        
        person = new Person { Name = input.Name, EmailAddress = input.EmailAddress };
        _personRepository.Insert(person);
    }
}
```
## 自定义存储库
### 接口
```csharp?linenums
public interface IPersonRepository : IRepository<Person>
{

}
public interface IPersonRepository : IRepository<Person, long>
{

}
```
* IPersonRepository扩展了IRepository \<TEntity\>。它用于定义具有主键类型int（Int32）的实体。
* 如果您的实体的主键不是int，则可以如下所示扩展 IRepository \<TEntity，TPrimaryKey\>接口。
### 实现
仓库在NHibernate和 EntityFramework中是作为开箱即用的方式实现的。
### 基础知识库方法
#### 查询
##### 获取单一实体
```csharp?linenums
TEntity Get(TPrimaryKey id);
Task<TEntity> GetAsync(TPrimaryKey id);

TEntity Single(Expression<Func<TEntity, bool>> predicate);
Task<TEntity> SingleAsync(Expression<Func<TEntity, bool>> predicate);

TEntity FirstOrDefault(TPrimaryKey id);
Task<TEntity> FirstOrDefaultAsync(TPrimaryKey id);
TEntity FirstOrDefault(Expression<Func<TEntity, bool>> predicate);
Task<TEntity> FirstOrDefaultAsync(Expression<Func<TEntity, bool>> predicate);

TEntity Load(TPrimaryKey id);
```
* Get方法用于使用给定的主键（Id）获取实体。
* Single方法采用表达式获取单一实体。
* FirstOrDefault方法通过主键或者表达式。
* Load方法不从数据库检索实体，但为延迟加载创建代理对象。如果你只使用Id属性，Entity实际上没有被检索。只有在访问实体的其他属性时才从数据库中检索。出于性能的原因，可以使用这个来代替Get。如果ORM提供者没有实现它，Load方法和Get方法一样工作。
##### 获取实体列表
```csharp?linenums
List<TEntity> GetAllList();
Task<List<TEntity>> GetAllListAsync();

List<TEntity> GetAllList(Expression<Func<TEntity, bool>> predicate);
Task<List<TEntity>> GetAllListAsync(Expression<Func<TEntity, bool>> predicate);

IQueryable<TEntity> GetAll();
```
* GetAllList用于从数据库中检索所有实体。它的重载可以用来过滤实体。
* GetAll返回IQueryable \<T\>。所以，你可以在它之后添加Linq方法。
* 通过使用GetAll，几乎所有的查询都可以用Linq编写。即使它可以在连接表达式中使用。
##### 关于IQueryable \<T\>
从存储库方法调用GetAll（）时，必须有一个打开的数据库连接。这是因为延迟执行IQueryable \<T\>。它不会执行数据库查询，除非您调用ToList（）方法或在foreach循环中使用IQueryable\<T\>（或以某种方式访问​​查询的项目）。所以，当你调用ToList（）方法时，数据库连接必须是活着的。
#### 自定义返回值
还有一个额外的方法来提供可用于工作单元的IQueryable的功能。
```csharp?linenums
T Query<T>(Func<IQueryable<TEntity>, T> queryMethod);

var people = _personRepository.Query(q => q.Where(p => p.Name.Contains("H")).OrderBy(p => p.Name).ToList());
```
* Query方法接受一个接收IQueryable \<T\>的lambda（或方法）并返回任何类型的对象。
#### 插入

