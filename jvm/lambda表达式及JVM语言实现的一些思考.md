### lambda表达式及JVM语言实现的一些思考

`Java 8`是一个激动人心的版本。

在基本类库上，JDK的很多api都进行了很大的改进，性能也有了很大的提升，特别是`HashMap`和新的`Time API`。而语法层面，应该是Java 5之后，特性增加最多的一个版本，接口的default方法、静态方法等等，还有就是，`lambda表达式`，以及引申出来的stream API、函数式接口、函数引用等。

Java王国里`动词`的地位终于有了提升。

****

`注：`

Java王国的名词动词，这个典故来自大神`Steve Yegge`的博客：[Execution in the Kingdom of Nouns ](http://steve-yegge.blogspot.jp/2006/03/execution-in-kingdom-of-nouns.html)，附上一篇中文翻译：[名词王国里的死刑](https://my.oschina.net/justjavac/blog/68536)。

暑假拜读了`Steve Yegge`的`《程序员的呐喊》`，书中也有这篇博文，读后受益匪浅。

****

#### Lambda Expression 

`Lambda Expression`，是一个`函数式编程（Functional Programming）`的概念，基于数学中的`λ演算（Lambda Calculus`）（具体概念可查看这个[博客](http://cgnail.github.io/tags/#lambda%E6%BC%94%E7%AE%97)，了解一下 ），在理论中，lambda表达式就是`λ演算`，而涉及到特定的某个编程语言时，可以看做是`匿名函数`（或者`闭包`）。λ演算在数学和计算机领域都有着很重要的地位，它基于很简单的几条定理，而推导出来的结论却可以完成任何可计算问题，也就是说，λ演算很简单，但却是`图灵完备`的。很多的函数式编程语言都起源于λ演算，例如`Haskell`、`Lisp`及其方言`Scheme`、`Clojure`等等。

`λ演算`的`BNF范式`表达:

    <expression> ::= <identifier>
    <expression> ::= (λ <identifier> . <expression>)
    <expression> ::= (<expression> <expression>)

例子：

    (lambda x . plus x x) y

****

#### java 8中的lambda表达式

`java 8`中加入的lambda表达式，总体来说，还不成熟，限制还很多，但同时让人感到很惊艳，因为这次不是像`java泛型`那样的`语法糖`，而是真的在`字节码`上对lambda提供了支持。在java 8出来之前，人们一直猜测，`Oracle`可能只是用一个语法糖将`匿名内部类`包装成lambda表达式，但结果却是使用java 7添加的`invokedynamic`指令，让人感受到满满的诚意（却也依然不能改变因为Oracle告Google的Android侵权留下的坏印象）。这样做的好处是，以后对lambda表达式进行改进时，可以不像泛型那样受到兼容性的影响，而且，不采取匿名内部类的方式，可以提高很多的性能（因为使用匿名内部类的话，JVM进行类加载需要花费较多的时间）。

lambda表达式带来的不仅仅是语法层面的美观，对于代码的可读性也有很大的帮助。

以一个简单的打印列表内所有元素的例子为例。

在java 8之前，需要这么写：

    List<String> list=new ArrayList<>();
    //balabala,add some items
    //method one
    for(int i=0;i<list.size();i++){
        System.out.println(list.get(i));
    }
    //method two
    for(String li:list){
        System.out.println(li);
    }

而加入lambda后：

    //method one
    list.forEach(e->System.out.println(e));
    //method two,use function reference
    list.forEach(System.out::println);
    //done,cool!

是不是可以少很多代码，而且可读性也提高了不少？

当然，这种提升对于Python、JavaScript等语言来说可能根本不屑，但考虑到Java一直以来的臃肿和繁琐，引入函数式编程可以说是很大的一个进步了。

****

#### java 8的lambda表达式的局限性

前面说过，java 8的lambda表达式还不成熟，还有很多的局限性。

* lambda表达式只适用于`函数式接口`，就是显式或者非显式添加了`@FunctionalInterface`注解，且只有`一个`非静态、非default的函数的接口（例如`Runnable`）。就是说，要想真正使用lambda表达式，你必须先有一个函数式接口（好像还是只有名词的感觉，函数依然要依赖于类），无法做到真正的匿名函数。不过好在JDK里添加了许多内置的函数式接口在包`java.util.function`下，目前来说还是可以满足开发需求的。
* 在lambda内访问的变量必须是`final`的，或者说语义上是`final`的，也就是说，lambda表达式必须是语义上不改变外部变量的。
* 过度依赖于基本类库。`Stream API`，加强版的`Collections Framework`，等等，lambda的使用，还是很依赖于类库的实现，而不是语法。

总而言之，lambda表达式是一个很大的提升，但目前依然是：`语法跟不上脚步，靠类库硬撑`。

****

#### 其他语言的lambda 表达式比较

lambda表达式并不是函数式语言的专利，很多非函数式语言也加入了函数式编程的支持。

`C#`：C#应该是语法特性很多、写起来特别爽的一个语言（毕竟巨硬亲儿子）。在`3.0`版本中，C#加入了对lambda的支持。它的具体实现是怎样的我不怎么清楚，但是因为`CLR`原生支持`delegate`，所以其实现应该比Java的lambda优美得多吧。C#的lambda表达式，用的比较多，应该是和linq一起使用吧（简直不要太爽）。

例子：

    /*items is a list*/
    //linq
    var result=from item in items 
                where item.property>233
                select item;
    //lambda
    var result=items.Where(item=>item.property>233);
    
`C++`：`C++ 11`中加入了lambda表达式，但是我感觉有点鸡肋，基本都用不上，而且很复杂（可能是我太菜了）。

例子：

    int hhh=233;
    auto f=[x](int x){return hhh+x;};

`Python`：`Python`是个很优雅的语言，但是它的lambda表达式我真的很想吐槽，只是一个阉割版的：只能写一行……虽然在知乎上可以看到很多一行Python实现的各种神奇代码（链接：[一行 Python 能实现什么丧心病狂的功能？](https://www.zhihu.com/question/37046157)），但是平时你会去想这些trick吗？如果需要在lambda表达式中进行比较复杂的操作，就必须先写个复杂函数来进行这些操作。虽然Python有`函数对象`，但是这样设计lambda表达式真的好吗？

例子：

    list=[x for x in range(1,233)]
    for n in map(lambda x:x**2,list):
        print(n)
    
`Groovy`：`Groovy`是我学习的第二个JVM上的语言，语法很像Python，和java完全兼容，动态语言和静态语言相结合的写法比单纯的动态语言要好多了（动态语言的`类型推导`是个坑啊，所以目前IDE的代码提示都做不到很智能，即使是`PyCharm`这样的）。在Groovy中，lambda表达式是以闭包的形式体现的（JavaScript天生支持的hhh）。

例子：

    def list=1..10
    list.each({x->print x})
    //或者
    list.each{
        print it
    }

**函数式语言的实现。**

`Common Lisp`：这个是我学习的第一个函数式语言，一开始看着那些括号真的是让人头大，就像别人评价的，简直就像是直接在写`语法树`（呃，其实这个也算是Lisp的一个优点吧）。Lisp用的是`前缀表达式`，很像编译时生成的中间代码的`四元式`（`三元式`）。

感受一下这个语法树一般的语法：

    (+ 1 2 3 4)
    或者更像语法树的写法
    (+(+ (+ 1 2) 3)4)
    

****

#### JVM其他语言的一些实现以及思考

这个部分的灵感很大一部分来源于一篇博文：[The Dark Side Of Lambda Expressions in Java 8](http://blog.takipi.com/the-dark-side-of-lambda-expressions-in-java-8/)。虽然原文作者对于lambda表达式的看法其实是有一些错误的 （原博的评论可以看到），但对于JVM上其他语言的实现的思考却很不错。

> 
The JVM was built to be language agnostic in the sense that it can execute code written in any language, as long as it can be translated into bytecode. The bytecode specification itself is fully OO, and was designed to closely match the Java language. That means that bytecode compiled from Java source will pretty much resemble it structurally.
>
But the farther away you get from Java – the more that distance grows. When you look at Scala which is a functional language, the distance between the source code and the executed bytecode is pretty big. Large amounts of synthetic classes, methods and variables are added by the compiler to get the JVM to execute the semantics and flow controls required by the language.

Java是一个完全`面向对象`的语言（抛开它的`原始类型`不谈），而`JVM平台`的`字节码规范`也是完全面向对象，这就意味着，从Java源码编译到字节码时，`语义`和`结构`上基本上是相符合的，不会有太大改变。

但是像`Groovy`这种动态语言、`Clojure`这种Lisp方言、`Scala`这种完美结合面向对象和函数式编程思想的语言……从源码到到字节码的差异是很大的，就是说，你必须用面向对象的方法来实现动态语言、函数式语言的特性，这就需要添加很多中间层来抹平这两者间的差异，也就是说需要很复杂的运行时来支持这些特性。

以Groovy为例，查看出现异常时它的栈轨。

`test.groovy`:

    def check(String s) {
        if (s.equals("")) {
		    throw new IllegalArgumentException()
	    }
    }
    def hhh=["lambda","","groovy"]
    hhh.each{
	    check it
    }

以debug模式运行的`栈轨`：

    >groovy --debug test.groovy
    Caught: java.lang.IllegalArgumentException
    java.lang.IllegalArgumentException
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
        at org.codehaus.groovy.reflection.CachedConstructor.invoke(CachedConstructor.java:83)
        at org.codehaus.groovy.runtime.callsite.ConstructorSite$ConstructorSiteNoUnwrapNoCoerce.callConstructor(ConstructorSite.java:105)
        at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCallConstructor(CallSiteArray.java:60)
        at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:235)
        at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:239)
        at test.check(test.groovy:3)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:93)
        at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:325)
        at org.codehaus.groovy.runtime.metaclass.ClosureMetaClass.invokeMethod(ClosureMetaClass.java:384)
        at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1024)
        at org.codehaus.groovy.runtime.callsite.PogoMetaClassSite.callCurrent(PogoMetaClassSite.java:69)
        at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callCurrent(AbstractCallSite.java:166)
        at test$_run_closure1.doCall(test.groovy:10)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.codehaus.groovy.reflection.CachedMethod.invoke(CachedMethod.java:93)
        at groovy.lang.MetaMethod.doMethodInvoke(MetaMethod.java:325)
        at org.codehaus.groovy.runtime.metaclass.ClosureMetaClass.invokeMethod(ClosureMetaClass.java:294)
        at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1024)
        at groovy.lang.Closure.call(Closure.java:414)
        at groovy.lang.Closure.call(Closure.java:430)
        at org.codehaus.groovy.runtime.DefaultGroovyMethods.each(DefaultGroovyMethods.java:2030)
        at org.codehaus.groovy.runtime.DefaultGroovyMethods.each(DefaultGroovyMethods.java:2015)
        at org.codehaus.groovy.runtime.DefaultGroovyMethods.each(DefaultGroovyMethods.java:2056)
        at org.codehaus.groovy.runtime.dgm$162.invoke(Unknown Source)
        at org.codehaus.groovy.runtime.callsite.PojoMetaMethodSite$PojoMetaMethodSiteNoUnwrapNoCoerce.invoke(PojoMetaMethodSite.java:274)
        at org.codehaus.groovy.runtime.callsite.PojoMetaMethodSite.call(PojoMetaMethodSite.java:56)
        at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCall(CallSiteArray.java:48)
        at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:113)
        at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:125)
        at test.run(test.groovy:9)
        at groovy.lang.GroovyShell.runScriptOrMainOrTestOrRunnable(GroovyShell.java:263)
        at groovy.lang.GroovyShell.run(GroovyShell.java:518)
        at groovy.lang.GroovyShell.run(GroovyShell.java:507)
        at groovy.ui.GroovyMain.processOnce(GroovyMain.java:653)
        at groovy.ui.GroovyMain.run(GroovyMain.java:384)
        at groovy.ui.GroovyMain.process(GroovyMain.java:370)
        at groovy.ui.GroovyMain.processArgs(GroovyMain.java:129)
        at groovy.ui.GroovyMain.main(GroovyMain.java:109)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.codehaus.groovy.tools.GroovyStarter.rootLoader(GroovyStarter.java:109)
        at org.codehaus.groovy.tools.GroovyStarter.main(GroovyStarter.java:131)

用`groovyc`命令编译`test.groovy`脚本，得到`test.class`和`test$_run_closure1.class`。再用`javap`命令进行反编译，得到以下结果：

    >javap -p test.class
    Compiled from "test.groovy"
    public class test extends groovy.lang.Script {
        private static org.codehaus.groovy.reflection.ClassInfo $staticClassInfo;
        public static transient boolean __$stMC;
        private static java.lang.ref.SoftReference $callSiteArray;
        public test();
        public test(groovy.lang.Binding);
        public static void main(java.lang.String...);
        public java.lang.Object run();
        public java.lang.Object check(java.lang.String);
        protected groovy.lang.MetaClass $getStaticMetaClass();
        private static void $createCallSiteArray_1(java.lang.String[]);
        private static org.codehaus.groovy.runtime.callsite.CallSiteArray $createCallSiteArray();
        private static org.codehaus.groovy.runtime.callsite.CallSite[] $
        getCallSiteArray();
    }

    >javap -p test$_run_closure1.class
    Compiled from "test.groovy"
    public class test$_run_closure1 extends groovy.lang.Closure implements org.codehaus.groovy.runtime.GeneratedClosure {
        private static org.codehaus.groovy.reflection.ClassInfo $staticClassInfo;
        public static transient boolean __$stMC;
        private static java.lang.ref.SoftReference $callSiteArray;
        public test$_run_closure1(java.lang.Object, java.lang.Object);
        public java.lang.Object doCall(java.lang.Object);
        public java.lang.Object doCall();
        protected groovy.lang.MetaClass $getStaticMetaClass();
        private static void $createCallSiteArray_1(java.lang.String[]);
        private static org.codehaus.groovy.runtime.callsite.CallSiteArray $createCallSiteArray();
        private static org.codehaus.groovy.runtime.callsite.CallSite[] $getCallSiteArray();
    }

从中我们可以看出，groovy脚本在运行时，编译成的是一个继承自`groovy.lang.Script`的类，而其中的闭包则是编译为一个继承自`groovy.lang.Closure`的内部类，运行时，通过`org.codehaus.groovy.tools.GroovyStarter`的main方法启动，然后`类加载器`加载生成的类，然后运行。

从`debug模式`的栈轨可以看出，运行一个groovy脚本，其中经历了很多层次的调用，而主要的都是`groovy运行时（runtime）`的调用，可见groovy运行时的复杂程度.

这个给我们的思考是，如果`JVM`不在字节码层面进行妥协的话，那么在`JVM`上实现的语言就必须编写很复杂的运行时环境来支持其与完全面向对象的字节码的差异。


