RPC式服务时通过单个URI暴露所有的服务，内部服务不可寻址，也不可知内部服务之间的关系。REST式的服务，服务即可寻址，也是连同的。

![](/assets/rpc vs rest.png)

什么是内部服务寻址？

Role Fielding博士在其论文中提出了rest架构风格的4个特征：可寻址性、无状态性、链接与连通性、统一接口。

无状态性：stateless

