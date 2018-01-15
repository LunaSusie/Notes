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


