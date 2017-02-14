由于@transactional可以注解到方法上，如果方法调用就设计到多个事务的嵌套关系，这时候就设计事务的传播行为。传播行为对于被调用的方法来说存在以下几种情况：

1、必须在事务中运行，有则加入，没有创建（默认情况）

2、必须在新事务中运行，不管是否处在事务上下文中

3、必须在已有的事务中运行，否在跑出异常

4、必须在没事务的上下文中运行，否则抛出异常

5、如果外部环境有事务，那就用事务，没有那就不用

@Transactional\(propagation=Propagation.REQUIRED\)

如果有事务, 那么加入事务, 没有的话新建一个\(默认情况下\)

@Transactional\(propagation=Propagation.REQUIRES\_NEW\)

不管是否存在事务,都创建一个新的事务,原来的挂起,新的执行完毕,继续执行老的事务

@Transactional\(propagation=Propagation.NOT\_SUPPORTED\)

容器不为这个方法开启事务



@Transactional\(propagation=Propagation.MANDATORY\)

必须在一个已有的事务中执行,否则抛出异常

@Transactional\(propagation=Propagation.NEVER\)

必须在一个没有的事务中执行,否则抛出异常\(与Propagation.MANDATORY相反\)

@Transactional\(propagation=Propagation.SUPPORTS\)

如果其他bean调用这个方法,在其他bean中声明事务,那就用事务.如果其他bean没有声明事务,那就不用事务.

