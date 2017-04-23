title: jsp页面下载文件
categories: JavaEE
tags: jsp
date: 2016-10-25 19:40
---


目标：将文件放在某路径下（如D:/src.zip），在网页中点击超链接下载文件。


![演示.gif](http://upload-images.jianshu.io/upload_images/1837782-732ba175902294f2.gif?imageMogr2/auto-orient/strip)
<!-- more -->
准备文件：
> 1、index.jsp
> 2、download.jsp
> 3、待下载文件：“src.zip”(路径为D:/src.zip)
> 4、程序结构
![程序结构.png](http://upload-images.jianshu.io/upload_images/1837782-9b514b5f7e9af8de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- `index.jsp`


     <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
    
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
      <head>
              <title>首页</title>
      </head>
      <body>
              <a href="download.jsp">点击下载</a>
      </body>
    </html>
  
  - `download.jsp`


    <%@page language="java" contentType="application/x-msdownload"
        pageEncoding="gb2312"%>
    <%
        String filename = "src.zip";
        String filepath = "D:";
        response.setContentType("APPLICATION/OCTET-STREAM");
        response.setHeader("Content-Disposition", "attachment; filename=\""
                + filename + "\"");
    
        java.io.FileInputStream fileInputStream = new java.io.FileInputStream(
                filepath + filename);
    
        int i;
        while ((i = fileInputStream.read()) != -1) {
            out.write(i);
        }
        fileInputStream.close();
    %>
    
注：index.jsp文件很灵活，超链接可以替换为按钮等等；待下载文件可以多样化，压缩包、jsp文件、图片等等各种文件都可以。
