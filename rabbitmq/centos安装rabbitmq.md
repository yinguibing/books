**1、centos系统安装 epel**

```
   EPEL，即Extra Packages for Enterprise Linux的简称，是为企业级Linux提供的一组高质量的额外软件包，包括但不限于Red Hat Enterprise Linux \(RHEL\), CentOS and Scientific Linux \(SL\), Oracle Enterprise Linux \(OEL\)。\(关于 : EPEL\)
```

命令： yum -y install epel-release

总结：如果不使用epel库，erlang等依赖包无法安装。

**2、安装erlang**

sudo yum update

sudo yum install erlang

![](/assets/install erlang.png)

**3、安装rabbitmq**

```
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
yum install rabbitmq-server-3.6.8-1.noarch.rpm
```

这种方式安装失败，提示no availbale package!

最终方式是从官网下载rpm安装包进行安装：

执行命令：sudo yum install rabbitmq-server-3.6.8-1.el7.noarch.rpm

![](/assets/rabbitmq install.png)**4、配置开机启动**

命令：sudo chkconfig rabbitmq-server on

5、启动页面管理

sudo rabbitmq-plugins enable rabbitmq\_management

6、启动服务器

sudo service rabbitmq-server restart

7、页面访问

端口：15672  http://172.16.16.103:15672/





