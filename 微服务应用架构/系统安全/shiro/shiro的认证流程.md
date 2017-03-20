1 创建表单页面，提交用户名和密码

2 提供controller接口

3 securityUtils.getSbuject() ; //进入认证流程

//realm
    authentication token 转换为指定的token实现

    数据库查询用户信息

    根据用户的情况构建authenticcationInfo返回

    SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(admin, admin.getPassword(), getName());