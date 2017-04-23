title: 马士兵struts2视频笔记--第一天
categories: Framework
tags: java
date: 2017-3-23 12:00
---


# 开发准备
jdk(http://www.oracle.com/technetwork/java/javase/downloads/index.html)
myeclipse
tomcat(http://tomcat.apache.org/)
struts2(http://struts.apache.org/download.cgi)
Struts2开发文档(http://struts.apache.org/docs/getting-started.html)
<!--more-->
# 第一个案例—`Hello World`
## 1.新建`web project`项目
`File`-`new`-`web project`，输入项目名，如`sturts2_001`。
 
![2-1 新建项目.png](http://upload-images.jianshu.io/upload_images/1837782-b267435a9cf12d54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2.配置项目
###  1 将开发库导入lib文件夹下
[下载链接](http://download.csdn.net/detail/qq_30461621/9792881)
###  2 在src包下新建struts.xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
 http://struts.apache.org/dtds/struts-2.0.dtd">
<struts>
	<!-- 
    <constant name="struts.enable.DynamicMethodInvocation" value="false" />
    <constant name="struts.devMode" value="false" />
    <include file="example.xml"/>
    <package name="default" namespace="/" extends="struts-default">
        <default-action-ref name="index" />
        <action name="index">
            <result type="redirectAction">
                <param name="actionName">HelloWorld</param>
                <param name="namespace">/example</param>
            </result>
        </action>
    </package>
	 -->
    <!-- Add packages here -->
</struts>
```
###  3 修改struts.xml
在 <!-- Add packages here --> 下面添加：
```xml
<constant name="struts.devMode" value="true" />
	 <package name="default" namespace="/" extends="struts-default">
        <action name="hello">
            <result>
                /Hello.jsp
            </result>
        </action>
 </package>
```
> 说明：
>>①<constant name="struts.devMode" value="true" />开发模式
>> ②action name：访问的名称
>>③package：类似于java中的包，解决重名问题

###  4 修改web.xml文件为
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
     <welcome-file-list>
    <welcome-file>Hello.jsp</welcome-file>
  </welcome-file-list>
  
  <filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```
###  5 访问
新建Hello.jsp文件，然后部署项目，访问localhost:8080/struts2/hello即可访问到Hello.jsp页面

 

![图2-2 项目结构.png](http://upload-images.jianshu.io/upload_images/1837782-20743e7ee7ac95fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.原理
 

![图3-1 struts2 核心原理.png](http://upload-images.jianshu.io/upload_images/1837782-51612ff1a6c167b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# namespace
```xml
<package name="default" namespace="/" extends="struts-default">
        <default-action-ref name="index" />
        <action name="index">
            <result type="redirectAction">
                <param name="actionName">HelloWorld</param>
                <param name="namespace">/example</param>
            </result>
        </action>
    </package>
``` 
> namespace决定了action的访问路径，默认为""，可以接收所有路径的action
>namespace可以写为’/’，或者’/xxx’，或者’/xxx/yyy’，对应的action访问路径为’/index.action’‘/xxx/index.action’，或者’/xxx/yyy/index.action’。
 namespace最好也用模块来进行命名

# action
 

![图4-1 执行过程.png](http://upload-images.jianshu.io/upload_images/1837782-7d0878e26e9eaac3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

action不一定是servlet，可以是一个普通类。
 

![图4-2 程序结构.png](http://upload-images.jianshu.io/upload_images/1837782-ff3c91d4befd511c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`<action name="path" class="com.bjsxt.struts2.path.action.PathAction">`
“class”是类的路径。当class属性没有配置时，默认访问ActionSupport类。
*action的三种实现方法*
###①普通类，手写execute方法。缺点：容易出错。
```java
public class IndexAction1 {
	public String execute() {
		return "success";
	}
}
```
### ②实现Action类，重载execute方法。
```java
public class IndexAction2 implements Action {
	@Override
	public String execute() {
		return "success";
	}
}
```
###③继承已经实现好的类，有很多可以直接用方法。*推荐使用*
```java
public class IndexAction3 extends ActionSupport {
	@Override
	public String execute() {
		return "success";
	}
}
```
# path
struts2中的路径问题是根据action的路径而不是jsp路径来确定，所以尽量不要使用相对路径。
虽然可以用redirect方式解决，但redirect方式并非必要。 
解决办法非常简单，统一使用绝对路径。（在jsp中用request.getContextRoot方式来拿到webapp的路径） 
或者使用myeclipse经常用的，指定basePath。
 

![图5-1 程序结构.png](http://upload-images.jianshu.io/upload_images/1837782-60ad236d422f246d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1. path.jsp
```xml
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<%@taglib uri="/struts-tags" prefix="s" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<base href="<%=basePath%>" />
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<title>Insert title here</title>
</head>
<body>
struts2中的路径问题是根据action的路径而不是jsp路径来确定，所以尽量不要使用相对路径。<br />
<a href="index.jsp">index.jsp</a>
<br />
虽然可以用redirect方式解决，但redirect方式并非必要。
<br />
解决办法非常简单，统一使用绝对路径。（在jsp中用request.getContextRoot方式来拿到webapp的路径）
<br />
或者使用myeclipse经常用的，指定basePath
</body>
</html>
```
> 注：前面设置好了<base>标签，就可以使用绝对路径了。

## 2 index.jsp
```xml
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>

<%--
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
//在head中<base href>指定basePath
--%>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<title>Insert title here</title>
</head>
<body>
	<a href="path/path">路径问题说明</a>
</body>
</html>
```
#  ActionMethod_DMI_动态方法调用.
Action执行的时候并不一定要执行execute方法，也可以访问其他方法。
*访问方法有两种：*
①在配置文件中配置Action的时候用method=“”来指定执行指定方法
②url地址中动态指定（动态方法调用DMI）（推荐）
#  1 index.jsp页面
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
Action执行的时候并不一定要执行execute方法<br />
可以在配置文件中配置Action的时候用method=来指定执行哪个方法
也可以在url地址中动态指定（动态方法调用DMI）（推荐）<br />
	<a href="<%=context %>/user/userAdd">添加用户</a>
	<br />
	<a href="<%=context %>/user/user!add">添加用户</a>
	<br />
前者会产生太多的action，所以不推荐使用
	
</body>
</html>
```
> 说明：第一个超链接是普通方法，对应struts.xml中第一个action，需要配置method属性。
第二个超链接是动态方法调用，struts.xml文件不需要改动，推荐使用。

##  2 struts.xml文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <constant name="struts.devMode" value="true" />
    <package name="user" extends="struts-default" namespace="/user">
        <action name="userAdd" class="com.bjsxt.struts2.user.action.UserAction" method="add">
            <result>/user_add_success.jsp</result>
        </action>
        
        <action name="user" class="com.bjsxt.struts2.user.action.UserAction">
            <result>/user_add_success.jsp</result>
        </action>
    </package>
</struts>
```

## 3 UserAction.java
```java
package com.bjsxt.struts2.user.action;

import com.opensymphony.xwork2.ActionSupport;

public class UserAction extends ActionSupport {
	public String add() {
		return SUCCESS;
	}
}
```

