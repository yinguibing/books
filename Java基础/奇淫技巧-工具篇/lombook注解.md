新建了一个Class类，然后在其中设置了几个字段，最后还需要花费很多时间来建立getter和setter方法

lombok项目的产生就是为了省去我们手动创建getter和setter方法的麻烦，它能够在我们编译源码的时候自动帮我们生成getter和setter方法。即它最终能够达到的效果是：在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法.

安装步骤：

1.下载lombok.jar包[https://projectlombok.org/download.html](https://projectlombok.org/download.html)

```
2.运行Lombok.jar: Java -jar D:\software\lombok.jar D:\software\lombok.jar这是windows下lombok.jar所在的位置



    数秒后将弹出一框，以确认eclipse的安装路径



3.确认完eclipse的安装路径后，点击install/update按钮，即可安装完成



4.安装完成之后，请确认eclipse安装路径下是否多了一个lombok.jar包，并且其



配置文件eclipse.ini中是否 添加了如下内容: 



    -javaagent:lombok.jar 

    -Xbootclasspath/a:lombok.jar 



如果上面的答案均为true，那么恭喜你已经安装成功，否则将缺少的部分添加到相应的位置即可 



5.重启eclipse或myeclipse
```

![](/assets/lombok安装提示.png)

sts下： 

-vmargs

-Dosgi.requiredJavaVersion=1.8

-Xms40m

-Dosgi.module.lock.timeout=10

-Xverify:none

-Dorg.eclipse.swt.browser.IEVersion=10001

-Xmx1200m

-javaagent:lombok.jar

