title: jsp传参方法小结
categories: Java
tags: jsp
date: 2016-10-8 20:10
---

## jsp页面到jsp页面##

1.从a.jsp传递

   `<a href="b.jsp?test=aaa"/>//将参数值为aaa，参数名test的参数传递到b.jsp页面中`

2.在b.jsp接收

    <% String info=request.getParameter("test");
       System.out.println("test的值是"+test); %>
 <!-- more -->   
## jsp页面到servlet##
1.首先需要新建一个Servlet，包的层次结构如下：
  >     demo
  >     --src
  >     ----servlet
  >     ------test.java

2.配置web.xml
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http:// java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-      app_3_0.xsd" version="3.0">
      <servlet>
         <!--别名-->
         <servlet-name>b</servlet-name>
         <!--包名.类名-->
         <servlet-class>servlet.test</servlet-class>
      </servlet>
    
      <servlet-mapping>
         <!--别名-->
         <servlet-name>b</servlet-name>
         <!--访问映射名-->
         <url-pattern>/aa</url-pattern>
      </servlet-mapping>
    </web-app>   
```
> 注：
> 
>  `1.<servlet></servlet>`
> 
> &nbsp;&nbsp;在向servlet或JSP页面制定初始化参数或定制URL时，必须首先命名servlet或JSP页面。Servlet元素就是用来完成此项任务的。
> 
> `2.<servlet-mapping></servlet-mapping>`
> 
> &nbsp;&nbsp;服务器一般为servlet提供一个缺省的URL：`http://host/webAppPrefix/servlet/ServletName。`
> 
>   但是，常常会更改这个URL，以便servlet可以访问初始化参数或更容易地处理相对URL。在更改缺省URL时，使用servlet-mapping元素。

3.从a.jsp中传参

 `<a href="aa?test=123"/>//将参数值为123，参数名test的参数传递到aa映射的servlet，即test.java中`

4.在test.java中接收值

       String info=request.getParameter("test");
       System.out.println("test的值是"+test); 
    
