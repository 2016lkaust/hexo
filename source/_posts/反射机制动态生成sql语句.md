title: 反射机制动态生成sql语句
categories: Java
tags: java
date: 2017-2-17 12:40
---
本例用于熟悉反射机制，代码很简单，水平有限，个人的一些理解都加在注释中了。
<!-- more -->
## bean包下
### 1.User.java


```java
	 package bean;
    import util.TableFieldAnno;
    import util.TableNameAnno;

    /**
     * 所有用户信息
     * 
     * @author LOOK
     * 
     */
	@TableNameAnno("user")
	public class User {
		@TableFieldAnno(field = "user_id", primaryKey = true, defaultNull = false)
		private String user_id;// 学号
		@TableFieldAnno(field = "user_name")
		private String user_name;// 姓名
		@TableFieldAnno(field = "user_tel")
		private String user_tel;// 联系方式
		@TableFieldAnno(field = "user_type")
		private String user_type;// 身份类型
		@TableFieldAnno(field = "user_pw")
		private String user_pw;// 密码
		@TableFieldAnno(field = "user_isGraduate")
		private Boolean isGraduate;// 是否毕业

	public String getUser_id() {
		return user_id;
	}

	public void setUser_id(String user_id) {
		this.user_id = user_id;
	}

	public String getUser_name() {
		return user_name;
	}

	public void setUser_name(String user_name) {
		this.user_name = user_name;
	}

	public String getUser_tel() {
		return user_tel;
	}

	public void setUser_tel(String user_tel) {
		this.user_tel = user_tel;
	}

	public String getUser_type() {
		return user_type;
	}

	public void setUser_type(String user_type) {
		this.user_type = user_type;
	}

	public String getUser_pw() {
		return user_pw;
	}

	public void setUser_pw(String user_pw) {
		this.user_pw = user_pw;
	}

	public Boolean getIsGraduate() {
		return isGraduate;
	}

	public void setIsGraduate(Boolean isGraduate) {
		this.isGraduate = isGraduate;
	}

	}
```

## util包下
### TableNameAnno .java
```java
	package util;
	
	import java.lang.annotation.ElementType;
	import java.lang.annotation.Retention;
	import java.lang.annotation.RetentionPolicy;
	import java.lang.annotation.Target;
	/**
	 * 用于注解表名
	 * @author LOOK
	 *
	 */
	@Target(value = ElementType.TYPE)
	@Retention(RetentionPolicy.RUNTIME)
	public @interface TableNameAnno {
		String value();
	}
```
### TableFieldAnno 

```java
	package util;

	import java.lang.annotation.ElementType;
	import java.lang.annotation.Retention;
	import java.lang.annotation.RetentionPolicy;
	import java.lang.annotation.Target;

	/**
	 * 用于注解类字段的属性
	 * 
	 * @author LOOK
	 * 
	 */
	@Target(value = ElementType.FIELD)
	@Retention(RetentionPolicy.RUNTIME)
	public @interface TableFieldAnno {
	String field();// 字段名称

	boolean primaryKey() default false;// 是否是主键

	String type() default "varchar";// 类型

	int length() default 30;

	boolean defaultNull() default true;
	}
```

### TestSqlByAnno.java
```java
	package util;

	import java.lang.reflect.Field;

	@SuppressWarnings("all")
	public class TestSqlByAnno {
	public static void main(String[] args) {
		createSql("User");
	}

	/**
	 * 用于动态生成建表语句
	 * 
	 * @param type
	 */
	public static void createSql(String type) {// 传入类名
		try {
			Class clazz = Class.forName("bean." + type);
			// 获取类的注解
			TableNameAnno tableNameAnno = (TableNameAnno) clazz
					.getAnnotation(TableNameAnno.class);
			System.out.println("create table " + tableNameAnno.value() + "(");
			// 获取所有属性的注解，输出属性注解的属性
			Field[] fields = clazz.getDeclaredFields();
			TableFieldAnno tableFieldAnno=null;
			for (int i = 0; i < fields.length; i++) {
				//获取每个属性的注解
				tableFieldAnno = fields[i]
						.getAnnotation(TableFieldAnno.class);
				//输出每个注解的属性
				System.out.print(tableFieldAnno.field() + " "
						+ tableFieldAnno.type() + " ("
						+ tableFieldAnno.length() + ") ");
				if (tableFieldAnno.primaryKey()) {// 主键
					System.out.println("primary key,");
				} else if (i == fields.length - 1) {// 最后一行
					System.out.println("not null ");
				} else {// 其他行
					System.out.println("not null, ");
				}

			}
			System.out.println(") default charset='utf8';");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}

	}
	}
```
