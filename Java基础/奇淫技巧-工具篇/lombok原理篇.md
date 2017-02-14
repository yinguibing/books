### 三、原理分析   {#h3_2}

接下来进行lombok能够工作的原理分析，以Oracle的javac编译工具为例。  
 自从Java 6起，javac就支持“JSR 269 Pluggable Annotation Processing API”规范，只要程序实现了该API，就能在javac运行的时候得到调用。  
 举例来说，现在有一个实现了"JSR 269 API"的程序A,那么使用javac编译源码的时候具体流程如下：  
 1\)javac对源代码进行分析，生成一棵抽象语法树\(AST\)  
 2\)运行过程中调用实现了"JSR 269 API"的A程序  
 3\)此时A程序就可以完成它自己的逻辑，包括修改第一步骤得到的抽象语法树\(AST\)  
 4\)javac使用修改后的抽象语法树\(AST\)生成字节码文件  
 详细的流程图如下：  
![](http://static.oschina.net/uploads/img/201509/24190818_h0nL.jpg)  
 lombok本质上就是这样的一个实现了"JSR 269 API"的程序。在使用javac的过程中，它产生作用的具体流程如下：  
 1\)javac对源代码进行分析，生成一棵抽象语法树\(AST\)  
 2\)运行过程中调用实现了"JSR 269 API"的lombok程序  
 3\)此时lombok就对第一步骤得到的AST进行处理，找到@Data注解所在类对应的语法树\(AST\)，然后修改该语法树\(AST\)，增加getter和setter方法定义的相应树节点  
 4\)javac使用修改后的抽象语法树\(AST\)生成字节码文件

### 六、lombok的罪恶 {#h3_5}

使用lombok虽然能够省去手动创建setter和getter方法的麻烦，但是却大大降低了源代码文件的可读性和完整性，降低了阅读源代码的舒适度。

