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



