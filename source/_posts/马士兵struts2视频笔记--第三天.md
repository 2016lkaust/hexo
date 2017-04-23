title: 马士兵struts2视频笔记--第三天
categories: Framework
tags: java
date: 2017-3-25 12:00
---
```
11、访问web元素	
    11.1 index.jsp
    11.2 struts.xml利用通配符调用方法	
    11.3 LoginAction1.java	
    11.4 LoginAction2.java(常用)	
    11.5 LoginAction3.java	
    11.6 LoginAction4.java	
    11.7 user_login_success.jsp	
12、模块包含和默认action	
13、Result结果类型	
    13.1 index.jsp	
    13.2 struts.xml	
14、全局结果集	
15、动态结果集	
    15.1 action方法中的处理	
    15.2 struts.xml	
16、带参数的结果集	
    16.1 index.jsp	
    16.2 struts.xml	
    16.3 UserAction.java	
    16.4 user_success.jsp
17、OGNL
18、struts标签

```
<!--more-->

# 访问web元素
## 11.1 index.jsp
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
取得Map类型request,session,application,真实类型 HttpServletRequest, HttpSession, ServletContext的引用:
<ol>
	<li>前三者：依赖于容器</li>
	<li>前三者：IOC</li> (只用这种)
	<li>后三者：依赖于容器</li>
	<li>后三者：IOC</li>
</ol>
<br />
<form name="f" action="" method="post">
用户名：<input type="text" name="name"/>
密码：<input type="text" name="password"/>
<br />
<!-- 多个按钮提交同一个表单到不同action -->
<input type="button" value="submit1" onclick="javascript:document.f.action='login/login1';document.f.submit();" />
<input type="button" value="submit2" onclick="javascript:document.f.action='login/login2';document.f.submit();" />
<input type="button" value="submit3" onclick="javascript:document.f.action='login/login3';document.f.submit();" />
<input type="button" value="submit4" onclick="javascript:document.f.action='login/login4';document.f.submit();" />
</form>
	
</body>
</html>
```
## 11.2 struts.xml利用通配符调用方法
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <constant name="struts.devMode" value="true" />
    <package name="login" extends="struts-default" namespace="/login">
        <action name="login*" class="com.bjsxt.struts2.user.action.LoginAction{1}">
            <result>/user_login_success.jsp</result>
        </action>
    </package>
</struts>
```
## 11.2 LoginAction1.java
```java
package com.bjsxt.struts2.user.action;
import java.util.Map;
import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
/**
 * 依赖struts2容器
 * ActionContext.getContext()不是一个单例
* 前端可以用request、session、application对象取出
 * @author LOOK
 *
 */
public class LoginAction1 extends ActionSupport {
	private Map request;
	private Map session;
	private Map application;
	public LoginAction1() {
//Stack Content的值
		request = (Map)ActionContext.getContext().get("request");
		session = ActionContext.getContext().getSession();
		application = ActionContext.getContext().getApplication();
	}
	public String execute() {
		request.put("r1", "r1");
		session.put("s1", "s1");
		application.put("a1", "a1");
		return SUCCESS; 
	}
}
```
## 11.3 LoginAction2.java(常用)

![图11-1 LoginAction2.png](http://upload-images.jianshu.io/upload_images/1837782-8881369c72a3d8ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```java
package com.bjsxt.struts2.user.action;

import java.util.Map;

import org.apache.struts2.interceptor.ApplicationAware;
import org.apache.struts2.interceptor.RequestAware;
import org.apache.struts2.interceptor.SessionAware;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
/**常用
 * DI dependency injection依赖注入
* IoC inverse of control控制反转
 * @author LOOK
 *
 */
public class LoginAction2 extends ActionSupport implements RequestAware,SessionAware, ApplicationAware {
	
	private Map<String, Object> request;
	private Map<String, Object> session;
	private Map<String, Object> application;
	
	public String execute() {
		request.put("r1", "r1");
		session.put("s1", "s1");
		application.put("a1", "a1");
		return SUCCESS; 
	}

	@Override
	public void setRequest(Map<String, Object> request) {
		this.request = request;//注入
	}

	@Override
	public void setSession(Map<String, Object> session) {
		this.session = session;
	}

	@Override
	public void setApplication(Map<String, Object> application) {
		this.application = application;
	}
}
```

## 11.4 LoginAction3.java
```java
package com.bjsxt.struts2.user.action;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.apache.struts2.ServletActionContext;

import com.opensymphony.xwork2.ActionSupport;
/**
 * 基本不用
 * @author LOOK
 *
 */
public class LoginAction3 extends ActionSupport {
	
	private HttpServletRequest request;
	private HttpSession session;
	private ServletContext application;
	
	public LoginAction3() {
		request = ServletActionContext.getRequest();
		session = request.getSession();
		application = session.getServletContext();
	}
	
	public String execute() {
		request.setAttribute("r1", "r1");
		session.setAttribute("s1", "s1");
		application.setAttribute("a1", "a1");
		return SUCCESS; 
	}
	
}
```
## 11.5 LoginAction4.java
```java
package com.bjsxt.struts2.user.action;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.apache.struts2.interceptor.ServletRequestAware;

import com.opensymphony.xwork2.ActionSupport;
/**
 * 基本不用
 * @author LOOK
 *
 */
public class LoginAction4 extends ActionSupport implements ServletRequestAware {
	
	private HttpServletRequest request;
	private HttpSession session;
	private ServletContext application;
	
	
	
	public String execute() {
		request.setAttribute("r1", "r1");
		session.setAttribute("s1", "s1");
		application.setAttribute("a1", "a1");
		return SUCCESS; 
	}



	@Override
	public void setServletRequest(HttpServletRequest request) {
		this.request = request;
		this.session = request.getSession();
		this.application = session.getServletContext();
	}
	
}
```
## 11.6 user_login_success.jsp
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
	User Login Success!
	<br />
	<s:property value="#request.r1"/> | <%=request.getAttribute("r1") %> <br />
	<s:property value="#session.s1"/> | <%=session.getAttribute("s1") %> <br />
	<s:property value="#application.a1"/> | <%=application.getAttribute("a1") %> <br />
	<!-- 遍历所有，查找attr中的key为a1，s1，r1的值，尽量不用 -->
	<s:property value="#attr.a1"/><br />
	<s:property value="#attr.s1"/><br />
	<s:property value="#attr.r1"/><br />
	<s:debug></s:debug>
	<br />
</body>
</html>
```
# 12、模块包含和默认action
```xml
<struts>
	<constant name="struts.devMode" value="true" />
	<!—默认：将错误输入都归到index action下 -->
	<default-action-ref name="index"></default-action-ref>
	<!—包含：协作开发时常用于将各模块的配置合并 -->
	<include file="login.xml" />
</struts>
```
# 13、Result结果类型
（1） dispatcher
（2） redirect
（3） chain
（4） redirectAction
（5） freemarker
（6） httpheader
（7） stream
（8） velocity
（9） xslt
（10） plaintext
（11） tiles
注：（1）（2）只能跳转到页面，不能跳转到action，这两个最常用
Redirect：服务器端跳转
## 13.1 index.jsp
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
Result类型
<ol>
	<li><a href="r/r1">dispatcher</a></li>
	<li><a href="r/r2">redirect</a></li>
	<li><a href="r/r3">chain</a></li>
	<li><a href="r/r4">redirectAction</a></li>
	<li>freemarker</li>
	<li>httpheader</li>
	<li>stream</li>
	<li>velocity</li>
	<li>xslt</li>
	<li>plaintext</li>
	<li>tiles</li>
</ol>
	
</body>
</html>
```
## 13.2 struts.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <constant name="struts.devMode" value="true" />
    <package name="resultTypes" namespace="/r" extends="struts-default">
	    <action name="r1">
	    	<result type="dispatcher">/r1.jsp</result>
	    </action>
	    
	    <action name="r2">
	    	<result type="redirect">/r2.jsp</result>
	    </action>
	    
	    <action name="r3">
	    	<result type="chain">r1</result>
	    </action>
	    
	    <action name="r4">
	    	<result type="redirectAction">r2</result>
	    </action>
	    
    </package>
</struts>
```

# 14、全局结果集
当很多action调用的结果集相同时，可以使用全局结果集，供pageage中所有action使用（方法的返回值为mainpage）。
```xml
<global-results>
    		<result name="mainpage">/main.jsp</result>
</global-results>
```
如果两个action不在同一个包里时，
```xml
<package name="admin" namespace="/admin" extends="user">
    	<action name="admin" class="com.bjsxt.struts2.user.action.AdminAction">
    		<result>/admin.jsp</result>
    	</action>
</package>
```
即：在另一个需要使用全局结果集的包里加上`extends=”user”`.
# 15、动态结果集
## 15.1 action方法中的处理
```java
public String execute() throws Exception {
		if(type == 1) r="/user_success.jsp";
		else if (type == 2) r="/user_error.jsp";
		return "success";
	}
```
## 15.2 struts.xml
```xml
<!-- 用于struts配置文件中的OGNL表达式，不是EL表达式 -->
    	<!-- 动态跳转到不同文件 -->
	    <action name="user" class="com.bjsxt.struts2.user.action.UserAction">
	    	<result>${r}</result>
	    </action>	
```
根据r的返回值跳转到user_success.jsp或者user_error.jsp。
# 16、带参数的结果集

![图16-1 带参数的结果集.png](http://upload-images.jianshu.io/upload_images/1837782-b1326d2b77d642dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 16.1 index.jsp
```jsp
向结果传参数
<ol>
	<li><a href="user/user?type=1">传参数</a></li>
</ol>
```
## 16.2 struts.xml
```java
<action name="user" class="com.bjsxt.struts2.user.action.UserAction">
	    	<result type="redirect">/user_success.jsp?t=${type}</result>
	    </action>
```
## 16.3 UserAction.java
```java
package com.bjsxt.struts2.user.action;

import com.opensymphony.xwork2.ActionSupport;

public class UserAction extends ActionSupport {
	private int type;
	
	public int getType() {
		return type;
	}

	public void setType(int type) {
		this.type = type;
	}

	@Override
	public String execute() throws Exception {
		return "success";
	}

}
```
## 16.4 user_success.jsp
取出传递的参数
```jsp
User Success!
	from valuestack（直接向jsp发送请求，没有经过action，所以值栈为空）: <s:property value="t"/><br/>
	from actioncontext: <s:property value="#parameters.t"/>
	<s:debug></s:debug>
```


# 17、OGNL 
1.  Object Graph Navigation Language
2.  想初始化domain model，可以自己new，也可以传参数值，但这时候需要保持参数为空的构造方法
3.  其他参考ognl.jsp
4.  什么时候在stack中会有两个Action？chain
ognl.jsp
```jsp
<?xml version="1.0" encoding="GB18030" ?>
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<%@ taglib uri="/struts-tags" prefix="s" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030" />
<title>OGNL表达式语言学习</title>
</head>
<body>
	<ol>
		<li>访问值栈中的action的普通属性: username = <s:property value="username"/> </li>
		<li>访问值栈中对象的普通属性(get set方法)user.age：<s:property value="user.age"/> | <s:property value="user['age']"/> | <s:property value="user[\"age\"]"/> | wrong: <%--<s:property value="user[age]"/>--%></li>
		<li>访问值栈中对象的普通属性(get set方法)cat.friend.name：<s:property value="cat.friend.name"/></li>
		<li>访问值栈中对象的普通方法password.length()：<s:property value="password.length()"/></li>
		<li>访问值栈中对象的普通方法cat.miaomiao()：<s:property value="cat.miaomiao()" /></li>
		<li>访问值栈中action的普通方法m()：<s:property value="m()" /></li>
		<hr />
		<li>访问静态方法@com.bjsxt.struts2.ognl.S@s()：<s:property value="@com.bjsxt.struts2.ognl.S@s()"/></li>
		<li>访问静态属性@com.bjsxt.struts2.ognl.S@STR：<s:property value="@com.bjsxt.struts2.ognl.S@STR"/></li>
		<li>访问Math类的静态方法@@max(2,3)：<s:property value="@@max(2,3)" /></li>
		<hr />
		<li>访问普通类的构造方法new com.bjsxt.struts2.ognl.User(8)：<s:property value="new com.bjsxt.struts2.ognl.User(8)"/></li>
		<hr />
		<li>访问List  users:<s:property value="users"/></li>
		<li>访问List中某个元素    users[1]:<s:property value="users[1]"/></li>
		<li>访问List中元素某个属性的集合(所有user的age属性构成的集合)   users.{age}:<s:property value="users.{age}"/></li>
		<li>访问List中元素某个属性的集合中的特定值(后者常用)    users.{age}[0]|users[0].age:<s:property value="users.{age}[0]"/> | <s:property value="users[0].age"/></li>
		<li>访问Set:<s:property value="dogs"/></li>
		<li>访问Set中某个元素(无序，访问不到):<s:property value="dogs[1]"/></li>
		<li>访问Map:<s:property value="dogMap"/></li>
		<li>访问Map中某个元素:<s:property value="dogMap.dog101"/> | <s:property value="dogMap['dog101']"/> | <s:property value="dogMap[\"dog101\"]"/></li>
		<li>访问Map中所有的key:<s:property value="dogMap.keys"/></li>
		<li>访问Map中所有的value:<s:property value="dogMap.values"/></li>
		<li>访问容器的大小：<s:property value="dogMap.size()"/> | <s:property value="users.size"/> </li>
		<hr />
		<li>投影(过滤)this指循环遍历中符合条件的 当前对象：<s:property value="users.{?#this.age==1}[0]"/></li>
		<li>投影：<s:property value="users.{^#this.age>1}.{age}"/></li>
		<li>投影：<s:property value="users.{$#this.age>1}.{age}"/></li>
		<li>投影：<s:property value="users.{$#this.age>1}.{age} == null"/></li>
		<hr />
		<li>[]Value Stack Contents的从上往下的内容:<s:property value="[0].username"/></li>
		
	</ol>
	
	<s:debug></s:debug>
</body>
</html>
```
# 18、struts标签
## 1.         通用标签：
> 
a)         property
b)         set
      i. 默认为action scope，会将值放入request和ActionContext
     ii.  page、request、session、application
c)         bean
d)         include(对中文文件支持有问题，不建议使用，如需包含，改用jsp包含)
e)         param
f)          debug

## 2.         控制标签
> 
a)         if elseif else
b)         iterator
              i. collections map enumeration iterator array
c)         subset

## 3.         UI标签
> 
a)         theme
                         i.              simple xhtml(默认) css_xhtml ajax

## 4. $ # %的区别
> a)         $用于i18n和struts配置文件
b)         #取得ActionContext的值
c)         %将原本的文本属性解析为ognl，对于本来就是ognl的属性不起作用
         i.   参考<s:property 和 <s:include>

[其他参考文档](http://struts.apache.org/docs/guides.html)。
