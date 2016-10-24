### JVM�������һЩʹ�ü���

����JVM�ṩ��ǿ�������ػ��ƣ�ʹ����java��������ʱ�����Զ�̬�ؼ��ء�ж�ز�ͬ���ࡣJVM�ṩ�����ֲ�ͬ���������������Ҳ���Լ̳�ClassLoader��ʵ���Լ������������Tomcatʵ�����Լ�������������ز�ͬӦ�õ�����������������Ǻܴ������ԣ��������������ʱ�ĳ�ʼ������������ʵ��һЩ����Ȥ�ļ��ɡ�

#### �����

����`����`��JVM�����ļ����ص��ڴ棬�������ֽ���У�顢����ת���ͽ��������ʼ���Ȳ����������γɿ��Ա������ֱ��ʹ�õ�java���͵Ĺ��̡�

����JVM��һ�������ǣ�����������ʱ�����е�`.class`�ļ����ص��ڴ��У����ǲ�ȡ��̬���صķ�ʽ���ӳٵ���Ҫʱ�ټ��أ�����Windows����Ķ�̬���ӿ⣩������JVM�в�����ĳ�����Class���������ʱ��������ཫ�ᱻж�ء�

��������`JVM�淶`�����£���ַ��[Chapter 5. Loading, Linking, and Initializing](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html)��������صĹ��̿��Ա�ʾΪ��ͼ��ԭ������visio��㻭�ĳ�ͼ����

![](http://www.w-angler.com/static/images/2016-10-24_1F82C95299365025B6D6A0C7609CA95E.png)

����������һһ����һ��ÿ�����̣�

* `����(Loading)`��ͨ�����ȫ�޶�����ȡ����Ķ������ֽ�����
������ֽ���������ľ�̬�洢�ṹת��Ϊ������������ʱ���ݽṹ���ڶ�������һ������������java.lang.Class������Ϊ��������Щ���ݵķ�����ڡ����ز��ֿ���ͨ��JVM�Դ������������ɣ�Ҳ����ͨ���û��Զ�������������ɣ��ɴˣ��û�����������׶ν��кܶ�Ķ��ƣ�������`Tomcat`���Զ������������������`WEB-INF/lib`��`WEB-INF/classes`�µ��ࡣ
* `��֤(Verification)`�����ӽ׶εĵ�һ����ȷ��Class�ļ����ֽ����а�������Ϣ���ϵ�ǰ�������Ҫ�󣬲��Ҳ���Σ�����������İ�ȫ����JVM�淶�У��������ֻ�кܼ�����ͳ�����������У�ֻ�涨������֤����ʱ��Ҫ�׳�`java.lang.VerifyError`�������Ծ���ʵ��������JVM��ʵ�֣���һ�㶼�������Щ������
 + �ļ���ʽ��֤
 + Ԫ������֤
 + �ֽ�����֤
 + ����������֤
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
 + ��һ�ε���`java.lang.invoke.MethodHandle`ʵ��ʱ��
 + ͨ��������ƶ�����е���ʱ��
 + ��ʼ��һ���࣬�丸��δ��ʼ��ʱ��
 + ��ʼ��ʵ���˰���`�ǳ���`��`�Ǿ�̬`�����Ľӿڵ���ʱ��
 + ָ����ִ�����ࣨ��main�������Ǹ��ࣩ�����������ʱ��
* `ʹ��(Using)`����JVM��ʹ�ü��غõ��ࡣ
* `ж��(Unloading)`����JVM�в�����ĳ�����Class���������ʱ��������ཫ�ᱻж�ء�
* `����(Linking)`��������֤��׼�������������ʿ��ơ����ǽ׶Ρ�

#### ���������ClassLoader��

�������������һ�������������ļ���Java�����JVM����������ʱ��̬����������ࡣ����������Լ����ļ�ϵͳ�������������Դ�����ļ�������˵��������������ģ�ֻҪ��������ܹ��õ������ֽ������������ļ���ȡ���������أ������ܱ����ص�JVM�С�

������JVM�У�������Ĭ�ϵ����������
* `Bootstrap�������`�����������������ĺ�����⣬��java.lang.*�ȡ�����������������ĸ���������Bootstrap�������û���κθ�����������������ڵײ����ϵͳ�������������ʵ�ֵ�һ���֣�����û�м̳�java.lang.ClassLoader�ࡣ

* `Extension�������`�������������ί�и����ĸ���������Ҳ����Bootstrap������������û�гɹ����صĻ����ٴ�`jre/lib/ext`Ŀ¼�»���`java.ext.dirs`ϵͳ���Զ����Ŀ¼�¼����ࡣ��oracle��JVMʵ�����`sun.misc.Launcher$AppClassLoader`�࣬�̳���java.lang.ClassLoader�࣬

* `Application�������`��Ҳ��System����������������`CLASSPATH`���������м���ĳЩӦ����ص��࣬CLASSPATH��������ͨ����-classpath��-cp������ѡ�������壬������JAR�е�Manifest��classpath���ԡ�Application���������Extension����������Ӽ���������oracle��JVMʵ�����`sun.misc.Launcher$ExtClassLoader`�࣬�̳���java.lang.ClassLoader�ࡣ

�����������ϵ������������������һ�ֱȽ���������ͣ��߳��������������������Ͳ�׸�������Բ鿴��ƪ���ͣ�[�߳��������������](http://blog.csdn.net/zhoudaxia/article/details/35897057) ��

����Ȼ������������������Ȥ�ĵط�����������Щ�Դ������������������Щ�����Լ����Ƶĵط�����Ϊ��������������ֽ�������Դ��ֻҪ��ͨ����֤�Ϳ����ˡ����ԣ������ͨ���Զ��������������ӻ��������κ�һ���ط�����һ���ൽ�㱾�ص�JVM�У�����Լ��ؾ������ܵ��ֽ��룬�ڼ��صĹ����н��ܣ�����Ǿ����ǶԳƼ��ܣ�`RSA`��`ECC`�ȣ��Ļ���������ò����ε����޷����м��ܹ���Java���������������ʱ�����ݻ���������Ĳ�ͬ�����ز�ͬ���ൽJVM�С���

��������ػ���һ��ί�л��ƣ����ǣ�������һ����ʱ����������ĵ���˳����Bootstrap���������Extension���������Application����������Լ�����������������ǰ�������������ز��ˣ����ί�и���һ������������ز��ˣ�����׳�`ClassNotFoundException`��

��������������ͨ��һ���򵥵���������⣺

`MyLoader.java`

    import java.io.FileInputStream;
	import java.nio.ByteBuffer;
	import java.nio.channels.FileChannel;
	
	/**
	 * �Զ������������Ϊ�˼򵥣�����һ���ض����ࡣ<br>
	 * �Զ�����븲��<code>findClass</code>������
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
			//Nashorn��java 8��ӵ�JavaScript���棬����extĿ¼��
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

���������ʱ�ĳ�ʼ��˳��

#### ʹ�ü���






