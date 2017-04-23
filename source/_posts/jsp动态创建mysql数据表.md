title: jsp动态创建mysql数据表
categories: JavaEE
tags: 
- jsp
- mysql
- code
date: 2017-2-11
---


以下是我自己想到的实现方法，如果读者有更好的方法实现，恳请指点。
![指定.gif](http://upload-images.jianshu.io/upload_images/1837782-46aec85362e9486c.gif?imageMogr2/auto-orient/strip)

<!-- more -->
![数据表.gif](http://upload-images.jianshu.io/upload_images/1837782-cad4d456f859053f.gif?imageMogr2/auto-orient/strip)

## 1.简单的jsp表单
`index.jsp`获取希望创建的数据表名称和列数
```java
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
	String path = request.getContextPath();
	String basePath = request.getScheme() + "://"
			+ request.getServerName() + ":" + request.getServerPort()
			+ path + "/";
%>

<!DOCTYPE HTML>
<html>
<head>
<base href="<%=basePath%>">

<title>动态创建数据表</title>
<meta charset="utf-8">

</head>

<body>
	<form action="createTableSlt" method="post">
		表名<input type="text" name="tablename" /><br> 
        列数<input type="number" name="length" /> <input type="submit" value="提交" />
	</form>
</body>
</html>

```
## 2.servlet
### 2.1新建createTableSlt.java文件
```java
package servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import bean.Create;

public class createTableSlt extends HttpServlet {
	public createTableSlt() {
		super();
	}

	public void destroy() {
		super.destroy(); // Just puts "destroy" string in log
		// Put your code here
	}

	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doPost(request, response);
	}

	/**
	 * 获取传递来的数据表名和列数，调用创建数据表的方法
	 * 
	 * @param request
	 *            the request send by the client to the server
	 * @param response
	 *            the response send by the server to the client
	 * @throws ServletException
	 *             if an error occurred
	 * @throws IOException
	 *             if an error occurred
	 */
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html");
		request.setCharacterEncoding("utf-8");
		response.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();

        //以下是关键代码
		String tablename = request.getParameter("tablename");
		int length = Integer.valueOf(request.getParameter("length"));
		Create.createTable(tablename, length);

		out.print("<!DOCTYPE html><html><body>成功</body></html>");
	}

	public void init() throws ServletException {
	}

}

```
### 2.2修改web.xml文件
```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
	<servlet>
		<servlet-name>createTableSlt</servlet-name>
		<servlet-class>servlet.createTableSlt</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>createTableSlt</servlet-name>
		<url-pattern>/createTableSlt</url-pattern>
	</servlet-mapping>

</web-app>
```

## 3.为了省事，bean与连接数据库合并在一个类里写了
```java
package bean;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Create {
	static String user = "root";
	static String pw = "123456";
    //数据库名为tables
	static String url = "jdbc:mysql://localhost:3306/tables";

	/**
	 * 创建连接
	 * 
	 * @return
	 */
	public static Connection getConnection() {
		Connection connection = null;
		try {
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection(url, user, pw);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return connection;
	}

	/**
	 * 关闭连接
	 * 
	 * @param connection
	 */
	public static void close(Connection connection) {
		try {
			connection.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	/**
	 * 创建指定数据表名和属性总数的数据表
	 * 
	 * @param tablename
	 * @param length
	 */
	public static void createTable(String tablename, int length) {
		Connection connection = getConnection();
		String sql = sql(tablename, length);
		try {
			PreparedStatement psStatement = (PreparedStatement) connection
					.prepareStatement(sql);
			psStatement.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	/**
	 * 动态生成sql建表语句，数据表为指定名称、指定属性长度（属性类型为String，主键为自动增长的id）
	 * 
	 * @param tablename
	 * @param length
	 * @return sql
	 */
	public static String sql(String tablename, int length) {
		StringBuffer sql = new StringBuffer();
		sql.append("create table " + tablename + "(");
		sql.append("id int(10) not null primary key auto_increment,");
		String columns[] = columnsName(length);
		for (int i = 0; i < columns.length; i++) {
			if (i == columns.length - 1) {
				sql.append(columns[i] + " varchar(20) not null");
				break;
			}
			sql.append(columns[i] + " varchar(20) not null,");
		}
		sql.append(");");
		return sql.toString();
	}

	/**
	 * 动态生成形如column_A形式的列名
	 * 
	 * @param length
	 * @return
	 */
	public static String[] columnsName(int length) {
		String columnsName[] = new String[length];
		for (int i = 0; i < length; i++) {
			char a = (char) (65 + i);
			columnsName[i] = "columns_" + String.valueOf(a);
		}
		return columnsName;
	}

}


```

[源码](https://github.com/lukeaust/createTable.git)
