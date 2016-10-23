### JVM类加载与一些使用技巧

　　JVM提供了强大的类加载机制，使得在java程序运行时，可以动态地加载、卸载不同的类。JVM提供了三种不同的类加载器，我们也可以继承ClassLoader来实现自己的类加载器（Tomcat实现了自己类加载器来加载不同应用的依赖）。这给了我们很大的灵活性，而且利用类加载时的初始化动作，可以实现一些很有趣的技巧。

#### 类加载

　　`定义`：JVM把类文件加载到内存，并进行字节码校验、数据转换和解析、类初始化等操作，最终形成可以被虚拟机直接使用的java类型的过程。

　　JVM的一个特性是，它不是启动时把所有的`.class`文件加载到内存中，而是采取动态加载的方式，延迟到需要时再加载（类似Windows程序的动态链接库），而当JVM中不存在某个类的Class对象的引用时，则这个类将会被卸载。

　　根据`JVM规范`第五章（地址：[Chapter 5. Loading, Linking, and Initializing](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html)），类加载的过程可以表示为下图（原谅我用visio随便画的丑图）：

![](http://www.w-angler.com/static/images/2016-10-24_1F82C95299365025B6D6A0C7609CA95E.png)

　　接下来一一讲解一下每个过程：

* `加载(Loading)`　通过类的全限定名获取此类的二进制字节流→
将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构→在堆中生成一个代表这个类的java.lang.Class对象作为方法区这些数据的访问入口。加载部分可以通过JVM自带的类加载器完成，也可以通过用户自定义的类加载器完成，由此，用户可以在这个阶段进行很多的定制，例如在`Tomcat`中自定义了类加载器，加载`WEB-INF/lib`和`WEB-INF/classes`下的类。
* `验证(Verification)`　连接阶段的第一步，确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。在JVM规范中，这个部分只有很简略笼统的描述（几行，只规定了在验证错误时需要抛出`java.lang.VerifyError`），所以具体实现依赖于JVM的实现，但一般都会进行这些操作：
 + 文件格式验证
 + 元数据验证
 + 字节码验证
 + 符号引用验证
* `准备(Preparation)`　为类变量（static修饰）分配内存并设置初始值。这里的初始值并不是初始化的值，而是该变量的数据类型的默认零值。但有一个例外，如果这个类变量同时也被final修饰，那么在编译时，就会直接为这个常量赋上目标值。
* `解析(Resolution)`　虚拟机将常量池中的符号引用替换为直接引用，JVM规范中，规定的解析动作有:
 + 类或接口解析
 + 类字段解析
 + 类方法解析
 + 接口方法解析
 + 方法类型和方法句柄解析
 + 调用现场符号（Call site specifier）解析
* `访问控制(Access Control)`　根据访问控制符（`public`、`protected`、`private`以及默认的包访问权限），限制方法调用和子类的方法重写等的访问权限。
* `覆盖(Overriding)`　子类覆写父类方法（根据JVM实现，在`虚函数表`中指向子类的方法入口）。
* `初始化(Initialization)`　执行类构造器的`<clinit>()`方法，进行类变量和其他资源的初始化。Java虚拟机规范规定了有`6`种情况必须立即对类进行初始化（加载，验证，准备必须在此之前完成）:
 + 调用`new`、`getstatic`、`putstatic`、`invokestatic`指令时（使用new关键字创建对象、读取类的静态字段、设置类的静态字段、调用类的静态方法）；
 + 第一次调用`java.lang.invoke.MethodHandle`实例时；
 + 通过反射机制对类进行调用时；
 + 初始化一个类，其父类未初始化时。
 + If C is an interface that declares a non-abstract, non-static method, the initialization of a class that implements C directly or indirectly. 初始化实现了包含`非抽象`、`非静态`方法的接口的类时；
 + 指定的执行主类（含main方法的那个类）在虚拟机启动时。
* `使用(Using)`　在JVM中使用加载好的类。
* `卸载(Unloading)`　当JVM中不存在某个类的Class对象的引用时，则这个类将会被卸载。
* `连接(Linking)`　包含验证、准备、解析、访问控制、覆盖阶段。

#### 类加载器（ClassLoader）

当一个 JVM 启动的时候，Java 缺省开始使用如下三种类型类装入器：


启动（Bootstrap）类加载器：引导类装入器是用本地代码实现的类装入器，它负责将 <Java_Runtime_Home>/lib 下面的类库加载到内存中。由于引导类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的引用，所以不允许直接通过引用进行操作。


标准扩展（Extension）类加载器：扩展类加载器是由 Sun 的 ExtClassLoader（sun.misc.Launcher$ExtClassLoader） 实现的。它负责将

< Java_Runtime_Home >/lib/ext 或者由系统变量 java.ext.dir 指定位置中的类库加载到内存中。开发者可以直接使用标准扩展类加载器。


系统（System）类加载器：系统类加载器是由 Sun 的 AppClassLoader（sun.misc.Launcher$AppClassLoader）实现的。它负责将系统类路径（CLASSPATH）中指定的类库加载到内存中。开发者可以直接使用系统类加载器。


除了以上列举的三种类加载器，还有一种比较特殊的类型就是线程上下文类加载器

a. Bootstrap ClassLoader/启动类加载器

主要负责jdk_home/lib目录下的核心 api 或 -Xbootclasspath 选项指定的jar包装入工作.

b. Extension ClassLoader/扩展类加载器

主要负责jdk_home/lib/ext目录下的jar包或 -Djava.ext.dirs 指定目录下的jar包装入工作

c. System ClassLoader/系统类加载器

主要负责java -classpath/-Djava.class.path所指的目录下的类与jar包装入工作.

d. User Custom ClassLoader/用户自定义类加载器(java.lang.ClassLoader的子类)

在程序运行期间, 通过java.lang.ClassLoader的子类动态加载class文件, 体现java动态实时类装入特性.

#### 类文件的结构

#### 类加载时的初始化

#### 使用技巧