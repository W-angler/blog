### lambda���ʽ��JVM����ʵ�ֵ�һЩ˼��

`Java 8`��һ���������ĵİ汾��

�ڻ�������ϣ�JDK�ĺܶ�api�������˺ܴ�ĸĽ�������Ҳ���˺ܴ���������ر���`HashMap`���µ�`Time API`�����﷨���棬Ӧ����Java 5֮��������������һ���汾���ӿڵ�default��������̬�����ȵȣ����о��ǣ�`lambda���ʽ`���Լ����������stream API������ʽ�ӿڡ��������õȡ�

Java������`����`�ĵ�λ��������������

****

`ע��`

Java���������ʶ��ʣ����������Դ���`Steve Yegge`�Ĳ��ͣ�[Execution in the Kingdom of Nouns ](http://steve-yegge.blogspot.jp/2006/03/execution-in-kingdom-of-nouns.html)������һƪ���ķ��룺[���������������](https://my.oschina.net/justjavac/blog/68536)��

��ٰݶ���`Steve Yegge`��`������Ա���ź���`������Ҳ����ƪ���ģ����������ǳ��

****

#### Lambda Expression 

`Lambda Expression`����һ��`����ʽ��̣�Functional Programming��`�ĸ��������ѧ�е�`�����㣨Lambda Calculus`�����������ɲ鿴���[����](http://cgnail.github.io/tags/#lambda%E6%BC%94%E7%AE%97)���˽�һ�� �����������У�lambda���ʽ����`������`�����漰���ض���ĳ���������ʱ�����Կ�����`��������`������`�հ�`��������������ѧ�ͼ�����������ź���Ҫ�ĵ�λ�������ںܼ򵥵ļ����������Ƶ������Ľ���ȴ��������κοɼ������⣬Ҳ����˵��������ܼ򵥣���ȴ��`ͼ���걸`�ġ��ܶ�ĺ���ʽ������Զ���Դ�ڦ����㣬����`Haskell`��`Lisp`���䷽��`Scheme`��`Clojure`�ȵȡ�

`������`��`BNF��ʽ`���:

    <expression> ::= <identifier>
    <expression> ::= (�� <identifier> . <expression>)
    <expression> ::= (<expression> <expression>)

���ӣ�

    (lambda x . plus x x) y

****

#### java 8�е�lambda���ʽ

`java 8`�м����lambda���ʽ��������˵���������죬���ƻ��ܶ࣬��ͬʱ���˸е��ܾ��ޣ���Ϊ��β�����`java����`������`�﷨��`�����������`�ֽ���`�϶�lambda�ṩ��֧�֡���java 8����֮ǰ������һֱ�²⣬`Oracle`����ֻ����һ���﷨�ǽ�`�����ڲ���`��װ��lambda���ʽ�������ȴ��ʹ��java 7��ӵ�`invokedynamic`ָ����˸��ܵ������ĳ��⣨ȴҲ��Ȼ���ܸı���ΪOracle��Google��Android��Ȩ���µĻ�ӡ�󣩡��������ĺô��ǣ��Ժ��lambda���ʽ���иĽ�ʱ�����Բ����������ܵ������Ե�Ӱ�죬���ң�����ȡ�����ڲ���ķ�ʽ��������ߺܶ�����ܣ���Ϊʹ�������ڲ���Ļ���JVM�����������Ҫ���ѽ϶��ʱ�䣩��

lambda���ʽ�����Ĳ��������﷨��������ۣ����ڴ���Ŀɶ���Ҳ�кܴ�İ�����

��һ���򵥵Ĵ�ӡ�б�������Ԫ�ص�����Ϊ����

��java 8֮ǰ����Ҫ��ôд��

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

������lambda��

    //method one
    list.forEach(e->System.out.println(e));
    //method two,use function reference
    list.forEach(System.out::println);
    //done,cool!

�ǲ��ǿ����ٺܶ���룬���ҿɶ���Ҳ����˲��٣�

��Ȼ��������������Python��JavaScript��������˵���ܸ�����м�������ǵ�Javaһֱ������ӷ�׺ͷ��������뺯��ʽ��̿���˵�Ǻܴ��һ�������ˡ�

****

#### java 8��lambda���ʽ�ľ�����

ǰ��˵����java 8��lambda���ʽ�������죬���кܶ�ľ����ԡ�

* lambda���ʽֻ������`����ʽ�ӿ�`��������ʽ���߷���ʽ�����`@FunctionalInterface`ע�⣬��ֻ��`һ��`�Ǿ�̬����default�ĺ����Ľӿڣ�����`Runnable`��������˵��Ҫ������ʹ��lambda���ʽ�����������һ������ʽ�ӿڣ�������ֻ�����ʵĸо���������ȻҪ�������ࣩ���޷�����������������������������JDK�������������õĺ���ʽ�ӿ��ڰ�`java.util.function`�£�Ŀǰ��˵���ǿ������㿪������ġ�
* ��lambda�ڷ��ʵı���������`final`�ģ�����˵��������`final`�ģ�Ҳ����˵��lambda���ʽ�����������ϲ��ı��ⲿ�����ġ�
* ���������ڻ�����⡣`Stream API`����ǿ���`Collections Framework`���ȵȣ�lambda��ʹ�ã����Ǻ�����������ʵ�֣��������﷨��

�ܶ���֮��lambda���ʽ��һ���ܴ����������Ŀǰ��Ȼ�ǣ�`�﷨�����ϽŲ��������Ӳ��`��

****

#### �������Ե�lambda ���ʽ�Ƚ�

lambda���ʽ�����Ǻ���ʽ���Ե�ר�����ܶ�Ǻ���ʽ����Ҳ�����˺���ʽ��̵�֧�֡�

`C#`��C#Ӧ�����﷨���Ժܶࡢд�����ر�ˬ��һ�����ԣ��Ͼ���Ӳ�׶��ӣ�����`3.0`�汾�У�C#�����˶�lambda��֧�֡����ľ���ʵ�����������Ҳ���ô�����������Ϊ`CLR`ԭ��֧��`delegate`��������ʵ��Ӧ�ñ�Java��lambda�����ö�ɡ�C#��lambda���ʽ���õıȽ϶࣬Ӧ���Ǻ�linqһ��ʹ�ðɣ���ֱ��Ҫ̫ˬ����

���ӣ�

    /*items is a list*/
    //linq
    var result=from item in items 
                where item.property>233
                select item;
    //lambda
    var result=items.Where(item=>item.property>233);
    
`C++`��`C++ 11`�м�����lambda���ʽ�������Ҹо��е㼦�ߣ��������ò��ϣ����Һܸ��ӣ���������̫���ˣ���

���ӣ�

    int hhh=233;
    auto f=[x](int x){return hhh+x;};

`Python`��`Python`�Ǹ������ŵ����ԣ���������lambda���ʽ����ĺ����²ۣ�ֻ��һ���˸��ģ�ֻ��дһ�С�����Ȼ��֪���Ͽ��Կ����ܶ�һ��Pythonʵ�ֵĸ���������루���ӣ�[һ�� Python ��ʵ��ʲôɥ�Ĳ���Ĺ��ܣ�](https://www.zhihu.com/question/37046157)��������ƽʱ���ȥ����Щtrick�������Ҫ��lambda���ʽ�н��бȽϸ��ӵĲ������ͱ�����д�����Ӻ�����������Щ��������ȻPython��`��������`�������������lambda���ʽ��ĺ���

���ӣ�

    list=[x for x in range(1,233)]
    for n in map(lambda x:x**2,list):
        print(n)
    
`Groovy`��`Groovy`����ѧϰ�ĵڶ���JVM�ϵ����ԣ��﷨����Python����java��ȫ���ݣ���̬���Ժ;�̬�������ϵ�д���ȵ����Ķ�̬����Ҫ�ö��ˣ���̬���Ե�`�����Ƶ�`�Ǹ��Ӱ�������ĿǰIDE�Ĵ�����ʾ�������������ܣ���ʹ��`PyCharm`�����ģ�����Groovy�У�lambda���ʽ���Ահ�����ʽ���ֵģ�JavaScript����֧�ֵ�hhh����

���ӣ�

    def list=1..10
    list.each({x->print x})
    //����
    list.each{
        print it
    }

**����ʽ���Ե�ʵ�֡�**

`Common Lisp`���������ѧϰ�ĵ�һ������ʽ���ԣ�һ��ʼ������Щ�������������ͷ�󣬾���������۵ģ���ֱ������ֱ����д`�﷨��`��������ʵ���Ҳ����Lisp��һ���ŵ�ɣ���Lisp�õ���`ǰ׺���ʽ`���������ʱ���ɵ��м�����`��Ԫʽ`��`��Ԫʽ`����

����һ������﷨��һ����﷨��

    (+ 1 2 3 4)
    ���߸����﷨����д��
    (+(+ (+ 1 2) 3)4)
    

****

#### JVM�������Ե�һЩʵ���Լ�˼��

������ֵ���кܴ�һ������Դ��һƪ���ģ�[The Dark Side Of Lambda Expressions in Java 8](http://blog.takipi.com/the-dark-side-of-lambda-expressions-in-java-8/)����Ȼԭ�����߶���lambda���ʽ�Ŀ�����ʵ����һЩ����� ��ԭ�������ۿ��Կ�������������JVM���������Ե�ʵ�ֵ�˼��ȴ�ܲ���

> 
The JVM was built to be language agnostic in the sense that it can execute code written in any language, as long as it can be translated into bytecode. The bytecode specification itself is fully OO, and was designed to closely match the Java language. That means that bytecode compiled from Java source will pretty much resemble it structurally.
>
But the farther away you get from Java �C the more that distance grows. When you look at Scala which is a functional language, the distance between the source code and the executed bytecode is pretty big. Large amounts of synthetic classes, methods and variables are added by the compiler to get the JVM to execute the semantics and flow controls required by the language.

Java��һ����ȫ`�������`�����ԣ��׿�����`ԭʼ����`��̸������`JVMƽ̨`��`�ֽ���淶`Ҳ����ȫ������������ζ�ţ���JavaԴ����뵽�ֽ���ʱ��`����`��`�ṹ`�ϻ�����������ϵģ�������̫��ı䡣

������`Groovy`���ֶ�̬���ԡ�`Clojure`����Lisp���ԡ�`Scala`������������������ͺ���ʽ���˼������ԡ�����Դ�뵽���ֽ���Ĳ����Ǻܴ�ģ�����˵����������������ķ�����ʵ�ֶ�̬���ԡ�����ʽ���Ե����ԣ������Ҫ��Ӻܶ��м����Ĩƽ�����߼�Ĳ��죬Ҳ����˵��Ҫ�ܸ��ӵ�����ʱ��֧����Щ���ԡ�

��GroovyΪ�����鿴�����쳣ʱ����ջ�졣

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

��debugģʽ���е�`ջ��`��

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

��`groovyc`�������`test.groovy`�ű����õ�`test.class`��`test$_run_closure1.class`������`javap`������з����룬�õ����½����

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

�������ǿ��Կ�����groovy�ű�������ʱ������ɵ���һ���̳���`groovy.lang.Script`���࣬�����еıհ����Ǳ���Ϊһ���̳���`groovy.lang.Closure`���ڲ��࣬����ʱ��ͨ��`org.codehaus.groovy.tools.GroovyStarter`��main����������Ȼ��`�������`�������ɵ��࣬Ȼ�����С�

��`debugģʽ`��ջ����Կ���������һ��groovy�ű������о����˺ܶ��εĵ��ã�����Ҫ�Ķ���`groovy����ʱ��runtime��`�ĵ��ã��ɼ�groovy����ʱ�ĸ��ӳ̶�.

��������ǵ�˼���ǣ����`JVM`�����ֽ�����������Э�Ļ�����ô��`JVM`��ʵ�ֵ����Ծͱ����д�ܸ��ӵ�����ʱ������֧��������ȫ���������ֽ���Ĳ��졣


