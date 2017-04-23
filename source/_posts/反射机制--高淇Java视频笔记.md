title: 反射机制--高淇java视频笔记

categories: Java

tags: java

date: 2017-2-15 20:05

---



![项目结构](/img/项目结构.png)

<!-- more -->

以注释形式讲解。

##  User.java

```java

	package bean;



	public class User {

	private int id;

	private int age;

	private String uname;



	public User() {



	}



	public User(int id, int age, String uname) {

		this.id = id;

		this.age = age;

		this.uname = uname;

	};



	public int getId() {

		return id;

	}



	public void setId(int id) {

		this.id = id;

	}



	public int getAge() {

		return age;

	}



	public void setAge(int age) {

		this.age = age;

	}



	public String getUname() {

		return uname;

	}



	public void setUname(String uname) {

		this.uname = uname;

	}



	}

```

##  Demo1.java

```java

	package test;



	/**

 	* 测试各种类型（class,interface,enum,annotation,void,primitive等）java.lang.Class对象的获取方法

 	* 

 	* @author LOOK

 	* 

 	*/

	@SuppressWarnings("all")

	public class Demo1 {

	public static void main(String[] args) {

		String path = "bean.User";

		try {

			// 对象是表示或封装一些数据。一个类被加载后，JVM会创建一个对应类的Class对象，类的整个结构信息会放到对应的Class对象中

			// 这个Class对象就像一面镜子，通过这面镜子可以看到对应类的全部信息

			Class clazz = Class.forName(path);

			System.out.println(clazz.hashCode());

			// 一个类只对应一个Class对象

			Class clazz1 = Class.forName(path);

			System.out.println(clazz1.hashCode());

			/*

			 * Class对象的三种获取方式

			 * 

			 * 1.Class.forName(string);

			 * 

			 * 2.类.class;

			 * 

			 * 3.对象.getClass()

			 */

			Class strClass = String.class;

			Class strClass2 = path.getClass();

			System.out.println(strClass == strClass2);

			

			/*

			 * 相同类型一样维数的Class对象一样，不同类型或者维数不同的不一样

			 */

			int[] arr01 = new int[10];

			int[] arr02 = new int[20];

			int[][] arr03 = new int[2][3];

			double[] arr04 = new double[3];

			System.out.println(arr01.getClass().hashCode());

			System.out.println(arr02.getClass().hashCode());

			System.out.println(arr03.getClass().hashCode());

			System.out.println(arr04.getClass().hashCode());

		} catch (ClassNotFoundException e) {

			e.printStackTrace();

		}

	}

	}

```



##  Demo2.java



```java

	package test;



	import java.lang.reflect.Constructor;

	import java.lang.reflect.Field;

	import java.lang.reflect.Method;

	

	/**

	 * 利用反射的API获取类的信息（类的名称、方法、属性、构造器等）

	 * 

	 * @author LOOK

	 * 

	 */

	@SuppressWarnings("all")

	public class Demo2 {

		public static void main(String[] args) {

		String path = "bean.User";

		try {

			Class clazz = Class.forName(path);



			/* 获取类的名字 */

			System.out.println(clazz.getName());// 获取包名+类名。bean.User

			System.out.println(clazz.getSimpleName());// 获取类名。User



			/* 获取类的属性 */

			Field[] fields = clazz.getFields();// 只能获取public类型的属性

			Field[] fields2 = clazz.getDeclaredFields();// 获取所有类型的属性

			Field field = clazz.getDeclaredField("id");// 获取指定名称的属性

			System.out.println(fields.length);

			System.out.println(fields2.length);

			System.out.println(field);

			for (Field temp : fields2) {

				System.out.println("属性：" + temp);

			}



			/* 获取类的方法 */

			Method[] methods = clazz.getDeclaredMethods();

			Method method01 = clazz.getDeclaredMethod("getUname", null);

			// 如果方法有参数，必须传递参数类型对应的class对象

			Method method02 = clazz.getDeclaredMethod("setUname", String.class);

			for (Method method : methods) {

				System.out.println(method);

			}



			/* 获取类的构造器信息 */

			Constructor[] constructors = clazz.getDeclaredConstructors();

			for (Constructor constructor : constructors) {

				System.out.println("构造器：" + constructor);

			}

			Constructor c01 = clazz.getDeclaredConstructor(null);

			System.out.println("无参构造器：" + c01);

			Constructor c02 = clazz.getConstructor(int.class, int.class,

					String.class);

			System.out.println("有参构造器：" + c02);



		} catch (ClassNotFoundException e) {

			e.printStackTrace();

		} catch (SecurityException e) {

			e.printStackTrace();

		} catch (NoSuchMethodException e) {

			e.printStackTrace();

		} catch (NoSuchFieldException e) {

			e.printStackTrace();

		}



	}

	}

```

##  Demo3.java

```java

	package test;

	

	import java.io.File;

	import java.lang.reflect.Constructor;

	import java.lang.reflect.Field;

	import java.lang.reflect.InvocationTargetException;

	import java.lang.reflect.Method;

	

	import bean.User;



	/**

	 * 通过反射API动态的操作：构造器、属性、方法

	 * 

	 * @author LOOK

	 * 

	 */

	@SuppressWarnings("all")

	public class Demo3 {

	public static void main(String[] args) {

		String path = "bean.User";

		try {

			Class clazz = Class.forName(path);



			/* 通过反射API 调用构造方法，构造对象 */

			User user = (User) (clazz.newInstance());// 其实是调用了无参构造器

			System.out.println(user);



			/* 调用有参构造器 */

			Constructor<User> c = clazz.getConstructor(int.class, int.class,

					String.class);

			User user2 = c.newInstance(1001, 22, "李四");

			System.out.println(user2.getAge());



			/* 通过反射API调用普通方法 */

			User user3 = (User) (clazz.newInstance());

			// 以下两行等价于user3.getUname();

			Method method = clazz.getDeclaredMethod("setUname", String.class);

			method.invoke(user3, "张三");

			System.out.println(user3.getUname());



			/* 通过反射API操作属性 */

			User user4 = (User) (clazz.newInstance());

			Field field = clazz.getDeclaredField("uname");

			field.setAccessible(true);// 该属性不做安全检查，可以直接修改。否则将报错。

			field.set(user4, "王五");// 通过反射直接写属性

			System.out.println(user4.getUname());// 通过反射直接读属性

		} catch (ClassNotFoundException e) {

			e.printStackTrace();

		} catch (SecurityException e) {

			e.printStackTrace();

		} catch (InstantiationException e) {

			e.printStackTrace();

		} catch (IllegalAccessException e) {

			e.printStackTrace();

		} catch (NoSuchMethodException e) {

			e.printStackTrace();

		} catch (IllegalArgumentException e) {

			e.printStackTrace();

		} catch (InvocationTargetException e) {

			e.printStackTrace();

		} catch (NoSuchFieldException e) {

			e.printStackTrace();

		}



	}

	}

```
