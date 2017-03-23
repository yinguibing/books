本文尝试阐述Javascript中的上下文与作用域背后的机制，主要涉及到执行上下文（execution context）、作用域链（scope chain）、闭包（closure）、this等概念。

**Execution context**

执行上下文（简称上下文）决定了Js执行过程中可以获取哪些变量、函数、数据，一段程序可能被分割成许多不同的上下文，每一个上下文都会绑定一个变量对象（variable object），它就像一个容器，用来存储当前上下文中所有已定义或可获取的变量、函数等。位于最顶端或最外层的上下文称为全局上下文（global context），全局上下文取决于执行环境，如Node中的global和Browser中的window：

![](http://mmbiz.qpic.cn/mmbiz_jpg/btsCOHx9LAMmWlcRC0IHTprgZqWw09r9uQZvpa6JtJibMBDlmDc2x3icNzqnL3ibbyODnM64rXFBmw1rtDXAoxlAQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)需要注意的是，上下文与作用域（scope）是不同的概念。Js本身是单线程的，每当有function被执行时，就会产生一个新的上下文，这一上下文会被压入Js的上下文堆栈（context stack）中，function执行结束后则被弹出，因此Js解释器总是在栈顶上下文中执行。在生成新的上下文时，首先会绑定该上下文的变量对象，其中包括arguments和该函数中定义的变量；之后会创建属于该上下文的作用域链（scope chain），最后将this赋予这一function所属的Object，这一过程可以通过下图表示：

![](http://mmbiz.qpic.cn/mmbiz_jpg/btsCOHx9LAMmWlcRC0IHTprgZqWw09r9xy0sAmMW32pPryA3SS3CbSuySkhGJoG0euUia53LJkTcSbwicIcxbxOg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)**this**

上文提到this被赋予function所属的Object，具体来说，当function是定义在global对中时，this指向global；当function作为Object的方法时，this指向该Object：

```js
var x = 1;  
var f = function(){  
  console.log(this.x);
}
f();  // -> 1

var ff = function(){  
  this.x = 2;
  console.log(this.x);
}
ff(); // -> 2  
x     // -> 2

var o = {x: "o's x", f: f};  
o.f(); // "o's x"
```

**Scope chain**

上文提到，在function被执行时生成新的上下文时会先绑定当前上下文的变量对象，再创建作用域链。我们知道function的定义是可以嵌套在其他function所创建的上下文中，也可以并列地定义在同一个上下文中（如global）。作用域链实际上就是自下而上地将所有嵌套定义的上下文所绑定的变量对象串接到一起，使嵌套的function可以“继承”上层上下文的变量，而并列的function之间互不干扰：

![](http://mmbiz.qpic.cn/mmbiz_jpg/btsCOHx9LAMmWlcRC0IHTprgZqWw09r9SINRgKZCK2TgL1TFksBibqPWro2cBVYAmq6x4Oe5MvDGWE7KicQI1VFw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

```js
var x = 'global';  
function a(){  
  var x = "a's x";
  function b(){
    var y = "b's y";
    console.log(x);
  };
  b();
}
function c(){  
  var x = "c's x";
  function d(){
    console.log(y);
  };
  d();
}
a();  // -> "a's x"  
c();  // -> ReferenceError: y is not defined  
x     // -> "global"  
y     // -> ReferenceError: y is not defined
```

**Closure**

如果理解了上文中提到的上下文与作用域链的机制，再来看闭包的概念就很清楚了。每个function在调用时会创建新的上下文及作用域链，而作用域链就是将外层（上层）上下文所绑定的变量对象逐一串连起来，使当前function可以获取外层上下文的变量、数据等。如果我们在function中定义新的function，同时将内层function作为值返回，那么内层function所包含的作用域链将会一起返回，即使内层function在其他上下文中执行，其内部的作用域链仍然保持着原有的数据，而当前的上下文可能无法获取原先外层function中的数据，使得function内部的作用域链被保护起来，从而形成“闭包”。看下面的例子：

```js
var x = 100;  
var inc = function(){  
  var x = 0;
  return function(){
    console.log(x++);
  };
};

var inc1 = inc();  
var inc2 = inc();

inc1();  // -> 0  
inc1();  // -> 1  
inc2();  // -> 0  
inc1();  // -> 2  
inc2();  // -> 1  
x;       // -> 100
```

执行过程如下图所示，inc内部返回的匿名function在创建时生成的作用域链包括了inc中的x，即使后来赋值给inc1和inc2之后，直接在global context下调用，它们的作用域链仍然是由定义中所处的上下文环境决定，而且由于x是在function inc中定义的，无法被外层的global context所改变，从而实现了闭包的效果：

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyBpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBXaW5kb3dzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkJDQzA1MTVGNkE2MjExRTRBRjEzODVCM0Q0NEVFMjFBIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkJDQzA1MTYwNkE2MjExRTRBRjEzODVCM0Q0NEVFMjFBIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QkNDMDUxNUQ2QTYyMTFFNEFGMTM4NUIzRDQ0RUUyMUEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QkNDMDUxNUU2QTYyMTFFNEFGMTM4NUIzRDQ0RUUyMUEiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6p+a6fAAAAD0lEQVR42mJ89/Y1QIABAAWXAsgVS/hWAAAAAElFTkSuQmCC)**this in closure**

我们已经反复提到执行上下文和作用域实际上是通过function创建、分割的，而function中的this与作用域链不同，它是由执行该function时当前所处的Object环境所决定的，这也是this最容易被混淆用错的一点。一般情况下的例子如下：

```js
var name = "global";  
var o = {  
  name: "o",
  getName: function(){
    return this.name
  }
};
o.getName();  // -> "o"
```

由于执行o.getName\(\)时getName所绑定的this是调用它的o，所以此时this == o；更容易搞混的是在closure条件下：

```js
var name = "global";  
var oo = {  
  name: "oo",
  getNameFunc: function(){
    return function(){
      return this.name;
    };
  }
}
oo.getNameFunc()();  // -> "global"  
```

此时闭包函数被return后调用相当于：

```

```

换一个更明显的例子：

```

```

当然，有时候为了避免闭包中的this在执行时被替换，可以采取下面的方法：

```

```

或者是在调用时强行定义执行的Object：

```

```

**总结**

Js是一门很有趣的语言，由于它的很多特性是针对HTML中DOM的操作，因而显得随意而略失严谨，但随着前端的不断繁荣发展和Node的兴起，Js已经不再是”toy language”或是jQuery时代的”CSS扩展”，本文提到的这些概念无论是对新手还是从传统Web开发中过度过来的Js开发人员来说，都很容易被混淆或误解，希望本文可以有所帮助。

