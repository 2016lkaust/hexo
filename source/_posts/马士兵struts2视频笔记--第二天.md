title: 马士兵struts2视频笔记--第二天
categories: Framework
tags: java
date: 2017-3-24 12:00
---

#7、通配符的使用
当类中方法的数量比较多时，方法中返回不同字符串，struts.xml中需要配置不同的<result name=”xxxx”></result>。使用通配符时，可以最大程度的减少配置，新建action和返回时的界面时，按照约定好的规则起名，配置文件不需要变更。
##7.1 index.jsp
```jsp
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>

<% String context = request.getContextPath(); %>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<title>Insert title here</title>
</head>
<body>
使用通配符，将配置量降到最低<br />
<a href="<%=context %>/actions/Studentadd">添加学生</a>
<a href="<%=context %>/actions/Studentdelete">删除学生</a>
<br />
不过，一定要遵守"约定优于配置"的原则
<br />
<a href="<%=context %>/actions/Teacher_add">添加老师</a>
<a href="<%=context %>/actions/Teacher_delete">删除老师</a>
<a href="<%=context %>/actions/Course_add">添加课程</a>
<a href="<%=context %>/actions/Course_delete">删除课程</a>
	
</body>
</html>
```
注：前两个超链接’Studentadd’、’Studentdelete’中’Student’后面的部分对应通配符.
## 7.2 struts.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <constant name="struts.devMode" value="true" />
    <package name="actions" extends="struts-default" namespace="/actions">
        <action name="Student*" class="com.bjsxt.struts2.action.StudentAction" method="{1}">
            <result>/Student{1}_success.jsp</result>
        </action>
        
        <action name="*_*" class="com.bjsxt.struts2.action.{1}Action" method="{2}">
            <result>/{1}_{2}_success.jsp</result>
            <!-- {0}_success.jsp -->
        </action>
    </package>
</struts>
```
注：①第一个action中，`{1}`对应第一个通配符’\*’，比如在`index.jsp`中点击`<a href="<%=context %>/actions/Studentadd">添加学生</a>`，tomcat查询`struts.xml`，其中\*对应`add` ，`{1}` 相当于`add`，动态拼接为字符串。
②第二个action中，’{1}’,’{2}’对应两个’\*’，点击’index.jsp’中的` <a href="<%=context %>/actions/Teacher_delete">删除老师</a>`时，访问的action等价于

```xml
<action name="*_*"  class="com.bjsxt.struts2.action.TeacherAction" method="delete">
            <result>/Teacher_delete_success.jsp</result>
```
# 8、接收参数的三种方法
## 8.1 用Action的属性接收参数
### 8.1.1 index.jsp中超链接使用action属性接收参数
`
<!--调用add方法，传递name和age属性-->
<a href="user/user!add?name=a&age=8">添加用户</a>
`
### 8.1.2 UserAction.java中接收
```java
package com.bjsxt.struts2.user.action;

import com.opensymphony.xwork2.ActionSupport;

public class UserAction extends ActionSupport {
	
	private String name;
	private int age;
	
	public String add() {
		System.out.println("name=" + name);
		System.out.println("age=" + age);
		return SUCCESS;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
}
```
## 8.2 用DomainModel接收参数(最常用)
与第一种相比，只是将属性封装到User类中，即model。
### 8.2.1 index.jsp
```jsp
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>

<% 
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<base href="<%=basePath %>"/>
<title>Insert title here</title>
</head>
<body> 
<!—传值方式改变，用“对象.属性”形式-->
使用Domain Model接收参数<a href="user/user!add?user.name=a&user.age=8">添加用户</a>
	
</body>
</html>
```
### 8.2.2 struts.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <constant name="struts.devMode" value="true" />
    <package name="user" extends="struts-default" namespace="/user">
        
        <action name="user" class="com.bjsxt.struts2.user.action.UserAction">
            <result>/user_add_success.jsp</result>
        </action>
    </package>
</struts>
```
### 8.2.3 UserAction.java
```java
package com.bjsxt.struts2.user.action;

import com.bjsxt.struts2.user.model.User;
import com.opensymphony.xwork2.ActionSupport;

public class UserAction extends ActionSupport {
	
	private User user;//传值时不需要生成user对象，struts会自动调用构造器
	
	public String add() {
		System.out.println("name=" + user.getName());
		System.out.println("age=" + user.getAge());
		return SUCCESS;
	}

	public User getUser() {
		return user;
	}

	public void setUser(User user) {
		this.user = user;
	}
	
}
```
### 8.2.4 User.java(新增文件)
```java
package com.bjsxt.struts2.user.model;

public class User {
	private String name;
	private int age;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
}
```

![图8-1 用DomainModel接收参数.png](http://upload-images.jianshu.io/upload_images/1837782-5399021fd9a37d93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 8.3 用ModelDriven接收参数
### 8.3.1 index.jsp和第一种传值一样。
```jsp
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>

<% 
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<base href="<%=basePath %>"/>
<title>Insert title here</title>
</head>
<body> 
使用ModelDriven接收参数<a href="user/user!add?name=a&age=8">添加用户</a>
	
</body>
</html>
```
### 8.3.2 UserAction.java
```java 
package com.bjsxt.struts2.user.action;

import com.bjsxt.struts2.user.model.User;
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;

public class UserAction extends ActionSupport implements ModelDriven<User>{
	//需要自己手动创建对象
	private User user = new User();
	
	public String add() {
		System.out.println("name=" + user.getName());
		System.out.println("age=" + user.getAge());
		return SUCCESS;
	}

	@Override
	public User getModel() {
		return user;
	}
	
}
```
其他文件和之前的一样。

![图 8-2用ModelDriven接收参数.png](http://upload-images.jianshu.io/upload_images/1837782-04f5d1ba6ed48209.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 9、中文乱码问题

** ①方法一：过滤器**
web.xml中配置过滤器为老版本的过滤器
```xml
<!-- <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class> -->
		<filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-class>
```
**②方法二：直接更改编码**
- jsp文件前加上
`<%@ page language="java" contentType="text/html; charset=UTF-8"
 pageEncoding="UTF-8" isELIgnored="false"%>`
同时，struts.xml文件中指定解码类型
` <constant name="struts.i18n.encoding" value="UTF-8"></constant>`
**注**
解决get传值乱码的方法：修改tomcat配置文件`server.xml`
`<Connector port="8080" protocol="HTTP/1.1" maxThreads="150" connectionTimeout="20000" redirectPort="8443" URIEncoding="UTF-8"/>`
#10、简单数据校验
## 10.1 添加验证
由8.1项目的基础上改进而来。在8.1.2 UserAction.add()方法里加上验证
```java
if(name == null || !name.equals("admin")) {
			this.addFieldError("name", "name is error");
			this.addFieldError("name", "name is too long");
			return ERROR;
}
```
注：这里addFieldError可以将错误信息反馈到前端(传统servlet中有request和response，但是struts中的action没有这两个对象)。
## 10.2 处理反馈信息
使用addFieldError方法和s:fieldError标签简单处理数据校验
```jsp
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<%@taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<title>Insert title here</title>
</head>
<body>
	User Add Error!错误信息的两种取法s:fielderror  
	<s:fielderror fieldName="name" theme="simple"/>
	<br />
s:property  
	<s:property value="errors.name[1]"/>
	<s:debug></s:debug>
</body>
</html>
```
## 10.3 struts.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <constant name="struts.devMode" value="true" />
    <package name="user" extends="struts-default" namespace="/user">
        <action name="user" class="com.bjsxt.struts2.user.action.UserAction">
            <result>/user_add_success.jsp</result>
            <result name="error">/user_add_error.jsp</result>
        </action>
    </package>
</struts>
```

![图10-1 运行结果.png](http://upload-images.jianshu.io/upload_images/1837782-9833fcc807c964ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

