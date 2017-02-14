Spring团队的建议是你在具体的类（或类的方法）上使用 @Transactional 注解，而不要使用在类所要实现的任何接口上。你当然可以在接口上使用 @Transactional 注解，但是这将只能当你设置了基于接口的代理时它才生效。因为注解是 不能继承 的，这就意味着如果你正在使用基于类的代理时，那么事务的设置将不能被基于类的代理所识别，而且对象也将不会被事务代理所包装（将被确认为严重的）。因 此，请接受Spring团队的建议并且在具体的类上使用 @Transactional 注解。

**事物超时设置:  
** @Transactional\(timeout=30\) //默认是30秒

**事务隔离级别:  
** @Transactional\(isolation = Isolation.READ\_UNCOMMITTED\)  
 读取未提交数据\(会出现脏读, 不可重复读\) 基本不使用  
 @Transactional\(isolation = Isolation.READ\_COMMITTED\)  
 读取已提交数据\(会出现不可重复读和幻读\)  
 @Transactional\(isolation = Isolation.REPEATABLE\_READ\)  
 可重复读\(会出现幻读\)  
 @Transactional\(isolation = Isolation.SERIALIZABLE\)  
 串行化

[MySQL](http://lib.csdn.net/base/14): 默认为REPEATABLE\_READ级别  
 SQLSERVER: 默认为READ\_COMMITTED

**脏读** : 一个事务读取到另一事务未提交的更新数据  
**不可重复读** : 在同一事务中, 多次读取同一数据返回的结果有所不同, 换句话说,   
 后续读取可以读到另一事务已提交的更新数据. 相反, "可重复读"在同一事务中多次  
 读取数据时, 能够保证所读数据一样, 也就是后续读取不能读到另一事务已提交的更新数据  
**幻读** : 一个事务读到另一个事务已提交的insert数据

