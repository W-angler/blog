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
* `初始化(Initialization)`　执行类构造器的`<clinit>()`方法，进行类变量和其他资源的初始化。Java虚拟机规范规定了有`6`种情况必须立即对类进行初始化（加载、验证、准备必须在此之前完成）:
 + 调用`new`、`getstatic`、`putstatic`、`invokestatic`指令时（使用new关键字创建对象、读取类的静态字段、设置类的静态字段、调用类的静态方法）；
 + 第一次调用`java.lang.invoke.MethodHandle`实例时；
 + 通过反射机制对类进行调用时；
 + 初始化一个类，其父类未初始化时。
 + 初始化实现了包含`非抽象`、`非静态`方法的接口的类时；
 + 指定的执行主类（含main方法的那个类）在虚拟机启动时。
* `使用(Using)`　在JVM中使用加载好的类。
* `卸载(Unloading)`　当JVM中不存在某个类的Class对象的引用时，则这个类将会被卸载。
* `连接(Linking)`　包含验证、准备、解析、访问控制、覆盖阶段。

#### 类加载器（ClassLoader）

　　类加载器是一个用来加载类文件的Java类或者JVM程序，在运行时动态加载所需的类。类加载器可以加载文件系统、网络或其他来源的类文件，就是说，不管你的类在哪，只要类加载器能够得到它的字节流（无论是文件读取、网络下载），就能被加载到JVM中。

　　在JVM中，有三类默认的类加载器：
* `Bootstrap类加载器`　它负责加载虚拟机的核心类库，如java.lang.*等。它是所有类加载器的父加载器。Bootstrap类加载器没有任何父类加载器，它依赖于底层操作系统，属于虚拟机的实现的一部分，它并没有继承java.lang.ClassLoader类。

* `Extension类加载器`将类加载请求先委托给它的父加载器，也就是Bootstrap类加载器，如果没有成功加载的话，再从`jre/lib/ext`目录下或者`java.ext.dirs`系统属性定义的目录下加载类。在oracle的JVM实现里，是`sun.misc.Launcher$AppClassLoader`类，继承自java.lang.ClassLoader类，

* `Application类加载器`　也叫System类加载器。它负责从`CLASSPATH`环境变量中加载某些应用相关的类，CLASSPATH环境变量通常由-classpath或-cp命令行选项来定义，或者是JAR中的Manifest的classpath属性。Application类加载器是Extension类加载器的子加载器。在oracle的JVM实现里，是`sun.misc.Launcher$ExtClassLoader`类，继承自java.lang.ClassLoader类。

　　除了以上的三种类加载器，还有一种比较特殊的类型，线程上下文类加载器，这里就不赘述，可以查看这篇博客：[线程上下文类加载器](http://blog.csdn.net/zhoudaxia/article/details/35897057) 。

　　然而，类加载这个部分有趣的地方，并不是这些自带的类加载器，而是那些可以自己定制的地方。因为类加载器并不管字节流的来源，只要能通过验证就可以了。所以，你可以通过自定义的类加载器，从互联网的任何一个地方加载一个类到你本地的JVM中；你可以加载经过加密的字节码，在加载的过程中解密，如果是经过非对称加密（`RSA`、`ECC`等）的话，便可以让不信任的人无法运行加密过的Java程序；你可以在运行时，根据环境、需求的不同，加载不同的类到JVM中……

　　类加载还有一个委托机制，就是，当加载一个类时，类加载器的调用顺序是Bootstrap类加载器→Extension类加载器→Application类加载器→自己定义的类加载器。当前面的类加载器加载不了，便会委托给下一级，如果都加载不了，便会抛出`ClassNotFoundException`。

　　接下来我们通过一个简单的例子来理解：

`MyLoader.java`

    import java.io.FileInputStream;
	import java.nio.ByteBuffer;
	import java.nio.channels.FileChannel;
	
	/**
	 * 自定义类加载器，为了简单，加载一个特定的类。<br>
	 * 自定义必须覆盖<code>findClass</code>方法。
	 * @author w-angler
	 *
	 */
	public class MyLoader extends ClassLoader {
		/**
		 * 获取字节流，假设在F盘下有一个hhh.class
		 * @param name
		 * @return
		 */
		private byte[] load(){
			try(FileInputStream is=new FileInputStream("F:\\hhh.class")){
				FileChannel channel=is.getChannel();
				ByteBuffer buffer=ByteBuffer.allocate((int)channel.size());
				channel.read(buffer);
				return buffer.array();
			}catch(Exception e) {
				e.printStackTrace();
				return null;
			}
		}
		@Override
		public Class<?> findClass(String name){
			byte[] b=load();
			return defineClass("hhh",b, 0, b.length);
		}
	}

`TestClassLoader.java`

    
	import javax.script.ScriptEngine;
	import javax.script.ScriptEngineManager;
	
	public class TestClassLoader {
		public static void main(String[] args) throws Exception {
			//Nashorn是java 8添加的JavaScript引擎，它在ext目录下
			ScriptEngineManager scriptEngineManager = new ScriptEngineManager(); 
			ScriptEngine nashorn = scriptEngineManager.getEngineByName("nashorn");
			MyLoader loader=new MyLoader();
			Class<?> cls=loader.loadClass("2333，这个参数其实没用了");
			System.out.println(String.class.getClassLoader());
			System.out.println(nashorn.getClass().getClassLoader());
			System.out.println(TestClassLoader.class.getClassLoader());
			System.out.println(cls.getClassLoader());
		}
	}

`结果：`

    null
	sun.misc.Launcher$ExtClassLoader@42a57993
	sun.misc.Launcher$AppClassLoader@73d16e93
	MyLoader@64bf3bbf

　　从结果可以看出，基本库中的类通过`Bootstrap类加载器`加载，所以输出为null；在ext目录下的类，通过`Extension类加载器`加载；在classpath中的类，通过`Application类加载器`；当这些类都无法加载时，采用自定义的类加载器（如果有）加载。

#### 类加载时的初始化

　　类加载时的初始化顺序

#### 使用技巧






