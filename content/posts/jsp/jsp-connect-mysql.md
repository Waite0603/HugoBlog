---
title: "JSP 访问数据库"
date: 2022-10-23T16:04:23+08:00
categories: ["Web"]
tags: ["Jsp"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---


# JSP 访问数据库

## mysql-connector-java的jar包下载

1. 官方下载地址 [https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/)

![](https://qiniu.waite.wang/202310121220594.png)

2. 解压, 把JDBC驱动放在lib文件夹下（直接复制过来就可以了）

![](https://qiniu.waite.wang/202310121223954.png)

> 记得导入, 不然会报错 

```java
<%@ page import="java.sql.*" %>
```

## 基本用法

### 连接MySQL数据库

在JSP中连接MySQL数据库的步骤如下：

1. 下载并安装MySQL数据库，启动MySQL服务。
2. 在MySQL中创建一个数据库和表，用于存储数据。可以使用MySQL自带的命令行工具或者图形化界面工具，例如phpMyAdmin等。
3. 在JSP中使用JDBC连接MySQL数据库，获取数据库连接对象。
4. 使用数据库连接对象创建Statement或者PreparedStatement对象，执行SQL语句。
5. 处理SQL执行结果，关闭数据库连接。
> 以下是一个简单的JSP连接MySQL数据库并查询数据的示例代码：

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
    // 导入JDBC相关的类
    <%@ page import="java.sql.*" %>
    // 定义数据库连接信息
    String url = "jdbc:mysql://localhost:3306/test";
    String user = "root";
    String password = "123456";
    Connection conn = null;
    Statement stmt = null;
    ResultSet rs = null;
    try {
        // 加载MySQL驱动程序
        Class.forName("com.mysql.jdbc.Driver");
        // 获取数据库连接
        conn = DriverManager.getConnection(url, user, password);
        // 创建Statement对象
        stmt = conn.createStatement();
        // 执行SQL查询语句
        String sql = "SELECT * FROM records";
        rs = stmt.executeQuery(sql);
        // 输出查询结果
        while (rs.next()) {
            out.println("id: " + rs.getInt("id") + "<br>");
            out.println("name: " + rs.getString("name") + "<br>");
            out.println("age: " + rs.getInt("age") + "<br>");
            out.println("<hr>");
        }
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        // 关闭数据库连接
        if (rs != null) {
            rs.close();
        }
        if (stmt != null) {
            stmt.close();
        }
        if (conn != null) {
            conn.close();
        }
    }
%>
```

> 在上面的示例中，我们首先定义了MySQL数据库的连接信息，包括数据库URL、用户名和密码。然后使用`Class.forName()`方法加载MySQL驱动程序，获取数据库连接对象，并使用`createStatement()`方法创建Statement对象，执行SQL查询语句，并使用`ResultSet`对象获取查询结果集，最后遍历结果集输出查询结果。最后，我们在`finally`代码块中关闭数据库连接。
>
> 需要注意的是，在实际开发中，我们应该将数据库连接信息配置在配置文件中，然后使用`Properties`对象读取配置文件中的信息，这样可以提高代码的可维护性和安全性。同时，我们也应该尽量避免在JSP页面中直接编写SQL语句，而是应该将数据库操作封装在Java类中，然后在JSP页面中调用这些Java类来执行数据库操作。

### 用结果集操作数据库中的表

在JSP中，我们可以使用`ResultSet`对象来操作数据库中的表，`ResultSet`对象表示一个结果集，它包含了一组查询结果的数据行。

> 在JSP中，我们可以使用`ResultSet`对象来操作数据库中的表，`ResultSet`对象表示一个结果集，它包含了一组查询结果的数据行。

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
    Connection conn = null;
    Statement stmt = null;
    ResultSet rs = null;
    try {
        // 获取数据库连接
        conn = getConnection();
        // 创建Statement对象
        stmt = conn.createStatement();
        // 执行查询语句
        String sql = "SELECT * FROM records";
        rs = stmt.executeQuery(sql);
        // 处理查询结果
        while (rs.next()) {
            int id = rs.getInt("id");
            String name = rs.getString("name");
            int age = rs.getInt("age");
            // 输出结果
            out.println("id=" + id + ", name=" + name + ", age=" + age + "<br>");
        }
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        // 关闭数据库连接
        if (rs != null) {
            rs.close();
        }
        if (stmt != null) {
            stmt.close();
        }
        if (conn != null) {
            conn.close();
        }
    }
%>
```

在上面的示例中，我们使用了一个名为`getConnection()`的方法来获取数据库连接，这个方法需要根据具体的数据库类型和配置来实现。另外，我们还使用了`Statement`对象来执行SQL查询语句，并使用`executeQuery()`方法获取查询结果的`ResultSet`对象。然后我们使用`ResultSet`对象的`next()`方法逐行读取查询结果，并使用`getInt()`和`getString()`等方法获取每一行的数据。

需要注意的是，在使用`ResultSet`对象操作数据库时，我们应该尽量避免在JSP页面中直接编写SQL语句，而是应该将数据库操作封装在Java类中，然后在JSP页面中调用这些Java类来执行数据库操作。这样可以提高代码的可维护性和安全性。
### 预处理语句

JSP中可以使用预处理语句（PreparedStatement）来执行SQL语句，预处理语句可以有效地防止SQL注入攻击，并提高数据库操作的效率。

预处理语句通常包含一个带有占位符的SQL语句和一组参数，占位符使用`?`表示。在执行预处理语句时，我们需要将参数设置到占位符中，然后执行预处理语句。

> 以下是一个简单的JSP预处理语句的示例代码：

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
    Connection conn = null;
    PreparedStatement stmt = null;
    ResultSet rs = null;
    try {
        // 获取数据库连接
        conn = getConnection();
        // 定义预处理语句
        String sql = "SELECT * FROM records WHERE id = ?";
        stmt = conn.prepareStatement(sql);
        // 设置参数
        stmt.setInt(1, 1);
        // 执行预处理语句
        rs = stmt.executeQuery();
        while (rs.next()) {
            // 处理查询结果
            int id = rs.getInt("id");
            String name = rs.getString("name");
            int age = rs.getInt("age");
            // 输出结果
            out.println("id=" + id + ", name=" + name + ", age=" + age + "<br>");
        }
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        // 关闭数据库连接
        if (rs != null) {
            rs.close();
        }
        if (stmt != null) {
            stmt.close();
        }
        if (conn != null) {
            conn.close();
        }
    }
%>
```

在上面的示例中，我们使用了一个名为`getConnection()`的方法来获取数据库连接，这个方法需要根据具体的数据库类型和配置来实现。另外，我们还使用了`PreparedStatement`对象来执行SQL语句，并使用`setInt()`方法将参数设置到占位符中。

需要注意的是，预处理语句通常比普通的SQL语句执行速度更快，因为预处理语句可以将SQL语句编译一次，然后多次执行，而普通的SQL语句每次执行都需要重新编译。另外，预处理语句也可以有效地防止SQL注入攻击，因为预处理语句会自动将参数进行转义。

另外，为了防止SQL注入攻击，我们应该将查询参数使用PreparedStatement来设置，而不是直接拼接SQL语句。例如：

```java
String sql = "SELECT * FROM records LIMIT ?, ?";
PreparedStatement stmt = conn.prepareStatement(sql);
stmt.setInt(1, start);
stmt.setInt(2, pageSize);
ResultSet rs = stmt.executeQuery();
```
