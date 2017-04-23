title: 解决表单post传递乱码
categories: JavaEE
tags: jsp
date: 2017-2-11
---

## 利用filter过滤器将字符编码转变为utf-8。

![过滤器位置](http://upload-images.jianshu.io/upload_images/1837782-0e9ff7ae5656b82b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<!-- more -->
## 1.新建EncodingFilter.java文件
```java
	package encoding;

	import java.io.*;
	import javax.servlet.*;

	public class EncodingFilter implements Filter {
	public void init(FilterConfig filterConfig) throws ServletException {

	}

	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		try {
			request.setCharacterEncoding("utf-8");
			response.setCharacterEncoding("utf-8");
		} catch (Exception e) {
		}

		chain.doFilter(request, response);
	}

	public void destroy() {

	}
	}
```

## 2.修改web.xml文件（如果没有就自己新建一个）
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
	<filter>
		<display-name>encoding</display-name>
		<filter-name>encoding</filter-name>
		<filter-class>encoding.EncodingFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	</web-app>
```
