JTA，即Java Transaction API，JTA允许应用程序执行分布式事务处理——在两个或多个网络计算机资源上访问并且更新数据。

[JDBC](http://baike.baidu.com/view/25611.htm)[驱动程序](http://baike.baidu.com/view/1048.htm)的JTA支持极大地增强了数据访问能力。

### JTA和JTS

Java

[事务](http://baike.baidu.com/view/121511.htm)

API（JTA：Java Transaction API）和它的同胞Java事务服务（JTS：Java Transaction Service），为J2EE平台提供了

[分布式事务](http://baike.baidu.com/view/262223.htm)

服务（distributed transaction）。

一个分布式事务（distributed transaction）包括一个事务管理器（transaction manager）和一个或多个

[资源管理器](http://baike.baidu.com/view/108140.htm)

\(resource manager）。

一个资源管理器（resource manager）是任意类型的持久化数据存储。

事务管理器（transaction manager）承担着所有事务参与单元者的相互通讯的责任。







### JTA与JDBC

JTA

[事务](http://baike.baidu.com/view/121511.htm)

比JDBC事务更强大。一个JTA事务可以有多个参与者，而一个JDBC事务则被限定在一个单一的数据库连接。下列任一个

[Java平台](http://baike.baidu.com/view/209634.htm)

的组件都可以参与到一个JTA事务中：JDBC连接、JDO PersistenceManager 对象、JMS 队列、JMS 主题、企业JavaBeans（EJB）、一个用J2EE Connector Architecture 规范编译的资源分配器。

