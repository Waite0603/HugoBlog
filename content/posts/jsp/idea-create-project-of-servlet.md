---
title: "Idea2023.2 创建 Servlet 项目"
date: 2022-10-23T16:04:23+08:00
categories: ["Web"]
tags: ["Jsp"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---



## Idea2023.2 创建 Servlet 项目

## 前期准备

+ 正常创建 Java 项目, 添加框架支持 Web 模块

  + 2023.2 之后 Idea 新 UI 把添加框架支持移到了 Ctrl + Alt + Shift + S(项目结构) -> 模块 -> +
  + 如果想要之前一样右键添加框架支持, 可以设置 -> 添加按键映射 -> 添加快捷键 -> 之后用快捷键使用即可

  ![](https://qiniu.waite.wang/202311021151150.png)

+ 项目结构 -> 模块 -> 添加模块

![](https://qiniu.waite.wang/202311021157709.png)

+ 修改项目结构

![](https://qiniu.waite.wang/202311021158842.png)

![](https://qiniu.waite.wang/202311021158575.png)

+ 配置 Tomcat, 可查看之前文章

## 开始

+ 在 src 右键创建 Servlet 文件
  + 如果右键new的时候没有servlet? 因为2023版的IDEA已经不支持Servlet了，但是如果还要使用的话，可以自己创建模板使用
  + 设置 -> 编辑器 -> 文件和代码模板

![image-20231102225812577](https://qiniu.waite.wang/202311022258626.png)

```java
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("File Header.java")
 
import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
 
@WebServlet("/${Class_Name}")
public class ${Class_Name} extends HttpServlet {
private String message;
 
public void init() {
 
}
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
 
}
 
@Override
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
this.doGet(request, response);
}
}
```

### 简单应用

> Hello World

```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    resp.setContentType("text/html");
    resp.getWriter().println("<h1>Welcome</h1>");
  }
}
```

> WEB-INF/ web.xml

```XML
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

<!-- 配置 Servlet -->
    <servlet>
        <servlet-name>helloServlet</servlet-name>
        <servlet-class>HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>helloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

> 过滤器

```java
import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;

@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {
  private String encoding;

  @Override
  public void init(FilterConfig filterConfig) throws ServletException {
    encoding = filterConfig.getInitParameter("encoding");
    if (encoding == null) {
      encoding = "UTF-8"; // 默认使用 UTF-8 编码
    }
  }

  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
      throws IOException, ServletException {
    request.setCharacterEncoding(encoding);
    response.setCharacterEncoding(encoding);
    chain.doFilter(request, response);
  }

  @Override
  public void destroy() {
    // 清理资源（如果有需要的话）
  }
}
```

> 项目结构如下

![image-20231102230026689](https://qiniu.waite.wang/202311022300860.png)