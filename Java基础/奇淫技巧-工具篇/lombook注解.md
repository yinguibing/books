新建了一个Class类，然后在其中设置了几个字段，最后还需要花费很多时间来建立getter和setter方法

lombok项目的产生就是为了省去我们手动创建getter和setter方法的麻烦，它能够在我们编译源码的时候自动帮我们生成getter和setter方法。即它最终能够达到的效果是：在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法.

![](/assets/lombok安装提示.png)

sts下： -vmargs -javaagent:lombok.jsr

