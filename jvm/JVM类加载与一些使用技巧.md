### JVM�������һЩʹ�ü���

����JVM�ṩ��ǿ�������ػ��ƣ�ʹ����java��������ʱ�����Զ�̬�ؼ��ء�ж�ز�ͬ���ࡣJVM�ṩ�����ֲ�ͬ���������������Ҳ���Լ̳�ClassLoader��ʵ���Լ������������Tomcatʵ�����Լ���������������ز�ͬӦ�õ�����������������Ǻܴ������ԣ��������������ʱ�ĳ�ʼ������������ʵ��һЩ����Ȥ�ļ��ɡ�

#### �����

����`����`��JVM�����ļ����ص��ڴ棬�������ֽ���У�顢����ת���ͽ��������ʼ���Ȳ����������γɿ��Ա������ֱ��ʹ�õ�java���͵Ĺ��̡�

����JVM��һ�������ǣ�����������ʱ�����е�`.class`�ļ����ص��ڴ��У����ǲ�ȡ��̬���صķ�ʽ���ӳٵ���Ҫʱ�ټ��أ�����Windows����Ķ�̬���ӿ⣩������JVM�в�����ĳ�����Class���������ʱ��������ཫ�ᱻж�ء�

��������`JVM�淶`�����£���ַ��[Chapter 5. Loading, Linking, and Initializing](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html)��������صĹ��̿��Ա�ʾΪ��ͼ��ԭ������visio��㻭�ĳ�ͼ����ͼ����һ����˳�򣬵�����Щ�׶�JVM�淶��δ�涨һ��Ҫ����������ֻ��ֱ���ϵ�˳��

![](http://www.w-angler.com/static/images/2016-10-24_1F82C95299365025B6D6A0C7609CA95E.png)

����������һһ����һ��ÿ�����̣�

* `����(Loading)`��ͨ�����ȫ�޶�����ȡ����Ķ������ֽ�����
������ֽ���������ľ�̬�洢�ṹת��Ϊ������������ʱ���ݽṹ���ڶ�������һ������������java.lang.Class������Ϊ��������Щ���ݵķ�����ڡ����ز��ֿ���ͨ��JVM�Դ������������ɣ�Ҳ����ͨ���û��Զ�������������ɣ��ɴˣ��û�����������׶ν��кܶ�Ķ��ƣ�������`Tomcat`���Զ������������������`WEB-INF/lib`��`WEB-INF/classes`�µ��ࡣ
* `��֤(Verification)`�����ӽ׶εĵ�һ����ȷ��Class�ļ����ֽ����а�������Ϣ���ϵ�ǰ�������Ҫ�󣬲��Ҳ���Σ�����������İ�ȫ����JVM�淶�У��������ֻ�кܼ�����ͳ�����������У�ֻ�涨������֤����ʱ��Ҫ�׳�`java.lang.VerifyError`�������Ծ���ʵ��������JVM��ʵ�֣���һ�㶼�������Щ������
 + �ļ���ʽ��֤����֤�ֽ����Ƿ����JVM�淶������Ҫ���㵱ǰJVM�İ汾��������
 + Ԫ������֤�����������������֤�ֽ��������Ƿ����Java�淶��
 + �ֽ�����֤������������������������֤����Ϸ����Ϻ��߼���
 + ����������֤���ڽ���������ת��Ϊֱ������ʱ����֤�Ϸ��ԣ���֤�����׶ε���ȷ�ԡ�
* `׼��(Preparation)`��Ϊ�������static���Σ������ڴ沢���ó�ʼֵ������ĳ�ʼֵ�����ǳ�ʼ����ֵ�����Ǹñ������������͵�Ĭ����ֵ������һ�����⣬�����������ͬʱҲ��final���Σ���ô�ڱ���ʱ���ͻ�ֱ��Ϊ�����������Ŀ��ֵ��
* `����(Resolution)`����������������еķ��������滻Ϊֱ�����ã�JVM�淶�У��涨�Ľ���������:
 + ���ӿڽ���
 + ���ֶν���
 + �෽������
 + �ӿڷ�������
 + �������ͺͷ����������
 + �����ֳ����ţ�Call site specifier������
* `���ʿ���(Access Control)`�����ݷ��ʿ��Ʒ���`public`��`protected`��`private`�Լ�Ĭ�ϵİ�����Ȩ�ޣ������Ʒ������ú�����ķ�����д�ȵķ���Ȩ�ޡ�
* `����(Overriding)`�����าд���෽��������JVMʵ�֣���`�麯����`��ָ������ķ�����ڣ���
* `��ʼ��(Initialization)`��ִ���๹������`<clinit>()`�����������������������Դ�ĳ�ʼ����Java������淶�涨����`6`�������������������г�ʼ�������ء���֤��׼�������ڴ�֮ǰ��ɣ�:
 + ����`new`��`getstatic`��`putstatic`��`invokestatic`ָ��ʱ��ʹ��new�ؼ��ִ������󡢶�ȡ��ľ�̬�ֶΡ�������ľ�̬�ֶΡ�������ľ�̬��������
 + ��ʹ��Java 7���ӵĶ�̬����֧��ʱ�����һ��`java.lang.invoke.MethodHandle`ʵ�����յĽ������Ϊ`REF_getStatic`��`REF_putStatic`��`REF_invokeStatic`�ķ������������������������Ӧ����û�н��г�ʼ��ʱ��
 + ͨ��������ƶ�����е��ã���δ����ʼ��ʱ��
 + ��ʼ��һ���࣬�丸��δ��ʼ��ʱ����ʼ�����ࣻ
 + ��ʼ��ʵ���˰���`�ǳ���`��`�Ǿ�̬`�����Ľӿڵ���ʱ���Ա���һ�£����Ӧ����Java 8���ӵģ�Ӧ����ָ`default����`����
 + ָ����ִ�����ࣨ��main�������Ǹ��ࣩ�����������ʱ��
* `ʹ��(Using)`����JVM��ʹ�ü��غõ��ࡣ
* `ж��(Unloading)`����JVM�в�����ĳ�����Class���������ʱ��������ཫ�ᱻж�ء�
* `����(Linking)`��������֤��׼�������������ʿ��ơ����ǽ׶Ρ�

#### ���������ClassLoader��

�������������һ�������������ļ���Java�����JVM�е�C++����������ʱ��̬����������ࡣ����������Լ����ļ�ϵͳ�������������Դ�����ļ�������˵��������������ģ�ֻҪ��������ܹ��õ������ֽ������������ļ���ȡ���������أ������ܱ����ص�JVM�С�

������JVM�У�������Ĭ�ϵ����������
* `Bootstrap�������`�����������������ĺ�����⣬��java.lang.*�ȡ�����������������ĸ���������Bootstrap�������û���κθ�����������������ڵײ����ϵͳ�������������ʵ�ֵ�һ���֣�����û�м̳�java.lang.ClassLoader�ࡣ

* `Extension�������`�������������ί�и����ĸ���������Ҳ����Bootstrap������������û�гɹ����صĻ����ٴ�`jre/lib/ext`Ŀ¼�»���`java.ext.dirs`ϵͳ���Զ����Ŀ¼�¼����ࡣ��oracle��JVMʵ�����`sun.misc.Launcher$AppClassLoader`�࣬�̳���java.lang.ClassLoader�࣬

* `Application�������`��Ҳ��System����������������`CLASSPATH`���������м���ĳЩӦ����ص��࣬CLASSPATH��������ͨ����-classpath��-cp������ѡ�������壬������JAR�е�Manifest��classpath���ԡ�Application���������Extension����������Ӽ���������oracle��JVMʵ�����`sun.misc.Launcher$ExtClassLoader`�࣬�̳���java.lang.ClassLoader�ࡣ

�����������ϵ������������������һ�ֱȽ���������ͣ��߳��������������������Ͳ�׸�������Բ鿴��ƪ���ͣ�[�߳��������������](http://blog.csdn.net/zhoudaxia/article/details/35897057) ��

����Ȼ������������������Ȥ�ĵط�����������Щ�Դ������������������Щ�����Լ����Ƶĵط�����Ϊ��������������ֽ�������Դ��ֻҪ��ͨ����֤�Ϳ����ˡ����ԣ������ͨ���Զ��������������ӻ��������κ�һ���ط�����һ���ൽ�㱾�ص�JVM�У�����Լ��ؾ������ܵ��ֽ��룬�ڼ��صĹ����н��ܣ�����Ǿ����ǶԳƼ��ܣ�`RSA`��`ECC`�ȣ��Ļ���������ò����ε����޷����м��ܹ���Java���������������ʱ�����ݻ���������Ĳ�ͬ�����ز�ͬ���ൽJVM�У��㻹����ͨ��ʹ�ò�ͬ�������������ͬһ�����ļ����������Բ���һЩ�����Ч������������ļ������μ��ز��������ǲ�ͬ�ģ�����

��������ػ���һ��`˫��ί�л���`�����ǣ�������һ����ʱ����������ĵ���˳����Bootstrap���������Extension���������Application����������Լ����������������������γ���һ����״�ṹ������`Bootstrap�������`�⣬����һ�����ڵ㣨�������ļ̳�����һ���������˫�ײ����Ǽ̳еģ�����Ȥ�Ŀ��Բ鿴`Open JDK`�е�ʵ��,��ʵ�ܼ򵥣����ӣ�[ClassLoader.java](http://hg.openjdk.java.net/jdk8u/jdk8u/jdk/file/f7be58eb30bc/src/share/classes/java/lang/ClassLoader.java)������ǰ�������������ز��ˣ����ί�и���һ������������ز��ˣ�����׳�`ClassNotFoundException`��

����ʵ���Լ�����������ܼ򵥣�ֻ��Ҫ�̳�`java.lang.ClassLoader`�����Ҹ���`findClass`����`loadClass`�����ͺ��ˡ�����ܻ��ʣ�Ϊʲô������������ѡ��������漰��java�ļ����Ժ���ʷ�ˡ�ClassLoader��Java����İ汾�о����ˣ����ǣ�ί�л�������1.2�ż���ģ��ڴ�֮ǰ���Զ������������ͨ������`loadClass`ʵ�ֵģ���֮��Ϊ�˼����ԣ��������`findClass`�����û����ǣ������Ϳ��Ա�֤ί�л��ơ��������ڣ�����`loadClass`����������ʱ����ί�У�������`findClass`�����ί�У����ز�����ͨ���Զ�����������ء����ڲ�����ͨ������`loadClass`�ķ�����ʵ�֡����ί�л�����ʵ����һЩ���⣬����Ͳ�ϸ˵�ˣ������������`Javaģ�黯`�������Ϣ��

��������������ͨ��һ���򵥵���������⣺

`MyLoader.java`

    import java.io.FileInputStream;
	import java.nio.ByteBuffer;
	import java.nio.channels.FileChannel;
	
	/**
	 * �Զ������������Ϊ�˼򵥣�����һ���ض����ࡣ<br>
	 * �Զ�����븲��<code>findClass</code>����<code>loadClass</code>������
	 * @author w-angler
	 *
	 */
	public class MyLoader extends ClassLoader {
		/**
		 * ��ȡ�ֽ�����������F������һ��hhh.class
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
			//Nashorn��Java 8��ӵ�JavaScript���棬����extĿ¼��
			ScriptEngineManager scriptEngineManager = new ScriptEngineManager(); 
			ScriptEngine nashorn = scriptEngineManager.getEngineByName("nashorn");
			MyLoader loader=new MyLoader();
			Class<?> cls=loader.loadClass("2333�����������ʵû����");
			System.out.println(String.class.getClassLoader());
			System.out.println(nashorn.getClass().getClassLoader());
			System.out.println(TestClassLoader.class.getClassLoader());
			System.out.println(cls.getClassLoader());
		}
	}

`�����`

    null
	sun.misc.Launcher$ExtClassLoader@42a57993
	sun.misc.Launcher$AppClassLoader@73d16e93
	MyLoader@64bf3bbf 

�����ӽ�����Կ������������е���ͨ��`Bootstrap�������`���أ��������Ϊnull����extĿ¼�µ��࣬ͨ��`Extension�������`���أ���classpath�е��࣬ͨ��`Application�������`������Щ�඼�޷�����ʱ�������Զ�����������������У����ء�

#### �����ʱ�ĳ�ʼ��

�����������ʱ�������һЩ��ʼ���Ĳ�����ǰ���ᵽ������`׼��(Preparation)`�׶Σ������һЩ��ʼ����������ʼ�������Ϊ��ֵ�����ں����ĳ�ʼ���У����Ծ�̬������ֵ������ִ�����е�`��̬��`��`static`�ؼ��������Ĵ���飩���ⲿ�ֵĳ�ʼ��ֻ���������ʱִ�У���ִֻ��һ�Ρ�

�������ڴ���һ����Ķ���ʱ��Ҳ�����һЩ��ʼ�������������������˵��һ��һ����Ӽ��ص�����һ�����󣬳�ʼ����˳��

1. ���ྲ̬�����;�̬�飬��������˳������ִ�У�ֻ�������ʱִ��һ�Σ���
2. ���ྲ̬�����;�̬��ʼ���飬��������˳������ִ�У�ֻ�������ʱִ��һ�Σ���
3. �����ʵ��������ʵ����ʼ���飬���ڴ����г��ֵ�˳������ִ�С�
4. ִ�и���Ĺ��췽����
5. ����ʵ��������ʵ����ʼ���飬���ڴ����г��ֵ�˳������ִ�С�
6. ִ������Ĺ��췽���� 

��������ʵ��һ�£�

	class A{
		static{
			System.out.println("super class' static block");
		}
		{
			System.out.println("super class' initialization block");
		}
		public A(){
			System.out.println("super class' constructor");
		}
	}
	class B extends A{
		static int i;
		static{
			System.out.println("subclass' static block");
			System.out.println(i);
		}
		static{
			i=233;
		}
		{
			System.out.println("subclass' initialization block");
		}
		public B(){
			System.out.println("subclass' constructor");
		}
	}
	
	public class Init {
		public static void main(String[] args){
			new B();
		}
	}

���������

    super class' static block
	subclass' static block
	0
	super class' initialization block
	super class' constructor
	subclass' initialization block
	subclass' constructor

�����ɴ˿ɼ���ʼ����˳���������ġ����У�������`B`�У���ӡ`i`�Ĵ�����ڸ�ֵ֮ǰ�����ԣ���ӡ����������׼���׶γ�ʼ������ֵ��������233��

#### ʹ�ü���

������ʵҲ˵����ʲô���ɣ�ֻ�Ƕ�����ػ��Ƶ�һЩӦ�á�

������һ�����ڻ������У��м����Զ����������������У���һ����`java.net.URLClassLoader`������ͨ����������ָ��Ŀ¼�µ��ࡣ�������Ϳ��Բ���Ҫ������ʱָ��classpath�ˣ����ǿ��Է���ĳ��Ŀ¼�£��Զ�����أ�ʵ�������ڲ���Ĺ��ܣ�Eclipse�������ʹ�����ַ�ʽ�ģ���������Բ鿴��ƪ���ͣ�[class�ļ��Ķ�̬����](http://www.iteye.com/topic/1113190)��

�����ڶ������������ʱ�������ֽ����Ľ��ܣ����ص��Ǽ��ܹ���class�ļ�������������е�ڰ��ˣ���Ҫ����ΪJava�����ױ������롣�����һЩ��ҵ���ܳ��򣬱��˻�ȡ�Ļ��������ױ�������Ȼ�����á����ԣ�����ͨ�����ַ�ʽ������ֹ�����������á��������㷨��ѡ�����Կ�Ĵ洢���ַ����Ͳ�����������۷�Χ�ˡ�

�������������Ҿ���ʹ�õģ�ʹ�þ�̬����������ļ��ļ��غͳ�ʼ����ʵ�ֳ���Ŀ����á���Web�����Լ�һЩ������򿪷��У������кܶ�����ã�����ɻ�ľ���Ӳ���뵽�����У�����һ����кܶ�������ļ���һ����`.ini`��`.xml`�ļ���java����һ��ר�Ÿ������`.properties`�ļ��Լ�`java.util.Properties`�࣬���Ҳ�����кܶ���json��Ϊ�����ļ���ģ��ģ�������������ļ��ļ���Ҳ��һ�����⣬���ȣ�ֻ����һ�Σ���Σ���ò�Ҫ�ֶ����ء����ԣ�һ����������ǣ��������ļ��ļ��غͽ������ھ�̬���У�����������ǰд�������������һ�����ã�

    
	import java.io.FileInputStream;
	import java.io.IOException;
	import java.util.Properties;

	import org.junit.Test;
	
	public final class Address {
		public static String ip;
		/**
		 * ��֤���ַ
		 */
		public static String captcha;
		/**
		 * ��¼��ַ
		 */
		public static String login;
		/**
		 * �ɼ�����ַ
		 */
		public static String subjects;
		/**
		 * ��ҳ,��ȡ����
		 */
		public static String name;
		static{
			try {
				FileInputStream in=new FileInputStream("ip.properties");
				Properties prop=new Properties();
				prop.load(in);
				
				ip=prop.getProperty("ip");
				captcha="http://"+ip+"/servlet/GenImg";
				login="http://"+ip+"/servlet/Login";
				subjects="http://"+ip+"/servlet/Svlt_QueryStuScore?year=0&term=&learnType=&scoreFlag=0";
				name="http://"+ip+"/stu/stu_index.jsp";
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		
		private Address(){
			throw new IllegalAccessError("You can not new an instance of this class");
		}
	}

��������������У���Ҫ���õ��ǽ������ϵͳ��IP���ж�����������磩������ֱ��ͨ��Address��������Щ���á���Ȼ������Ǻܼ򵥵����ã���Ҳ����ʵ��һЩ�����ӵġ�

****

������ƪ���ʹ���һ��ʼд����Ϊ�γ̡���ҵ�����⣬һֱû����ɣ�����󵱳���������뷨��û�ˣ���Щ�ط��Լ���������ȥ�ˡ�

****
