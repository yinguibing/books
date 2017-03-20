```
spring JPA 扩展的关键注解为@NoRepositoryBean，如果不使用此注解进行标注，会出现如下异常：
```

Caused by: org.hibernate.hql.internal.ast.QuerySyntaxException: Path expected for join! \[select new com.my118.model.example.ExamEntity\(ex1.id,ex1.content,ex1.type,ex2.note\) from com.my118.model.example.Example1Entity ex1 left join Example2Entity ex2 on ex1.type=ex2.typeid where ex1.type='5'\]

```
at org.hibernate.hql.internal.ast.QuerySyntaxException.convert\(QuerySyntaxException.java:74\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.hql.internal.ast.ErrorCounter.throwQueryException\(ErrorCounter.java:91\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.hql.internal.ast.QueryTranslatorImpl.analyze\(QueryTranslatorImpl.java:268\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.hql.internal.ast.QueryTranslatorImpl.doCompile\(QueryTranslatorImpl.java:190\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.hql.internal.ast.QueryTranslatorImpl.compile\(QueryTranslatorImpl.java:142\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.engine.query.spi.HQLQueryPlan.&lt;init&gt;\(HQLQueryPlan.java:115\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.engine.query.spi.HQLQueryPlan.&lt;init&gt;\(HQLQueryPlan.java:76\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.engine.query.spi.QueryPlanCache.getHQLQueryPlan\(QueryPlanCache.java:150\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.internal.AbstractSessionImpl.getHQLQueryPlan\(AbstractSessionImpl.java:302\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.internal.AbstractSessionImpl.createQuery\(AbstractSessionImpl.java:240\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.internal.SessionImpl.createQuery\(SessionImpl.java:1894\) ~\[hibernate-core-5.0.11.Final.jar:5.0.11.Final\]

at org.hibernate.jpa.spi.AbstractEntityManagerImpl.createQuery\(AbstractEntityManagerImpl.java:291\) ~\[hibernate-entitymanager-5.0.11.Final.jar:5.0.11.Final\]

... 208 common frames omitted
```

源代码如下

@Query\("select new com.my118.model.example.ExamEntity\(ex1.id,ex1.content,ex1.type,ex2.note\) from Example1Entity ex1, Example2Entity ex2 where ex1.type=ex2.typeid and ex1.type=:qType"\)

```java
public List&lt;ExamEntity&gt; findComData\(@Param\("qType"\) String qType\);
```

类注解为：



