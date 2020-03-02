# 1.父工程ssmbuild

<u>***【链接：https://blog.kuangstudy.com/index.php/archives/487/  】**</u>*

- 导入pom依赖，资源预留，编码：

  ```xml
  <dependencies>
          <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>javax.servlet-api</artifactId>
              <version>3.1.0</version>
          </dependency>
  
          <dependency>
              <groupId>javax.servlet.jsp</groupId>
              <artifactId>javax.servlet.jsp-api</artifactId>
              <version>2.2.1</version>
          </dependency>
  
          <dependency>
              <groupId>javax.servlet.jsp</groupId>
              <artifactId>jsp-api</artifactId>
              <version>2.2</version>
          </dependency>
  
          <dependency> <!--jsp表达式的依赖-->
              <groupId>javax.servlet.jsp.jstl</groupId>
              <artifactId>jstl-api</artifactId>
              <version>1.2</version>
          </dependency>
  
          <dependency> <!--jsp表达式的依赖的-->
              <groupId>taglibs</groupId>
              <artifactId>standard</artifactId>
              <version>1.1.2</version>
          </dependency>
  
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>8.0.16</version>
          </dependency>
  
          <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis</artifactId>
              <version>3.5.3</version>
          </dependency>
  
  
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
          </dependency>
  
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-webmvc</artifactId>
              <version>5.2.3.RELEASE</version>
          </dependency>
  
  
          <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>servlet-api</artifactId>
              <version>2.5</version>
          </dependency>
  
  
          <dependency>
              <groupId>com.fasterxml.jackson.core</groupId>
              <artifactId>jackson-databind</artifactId>
              <version>2.10.1</version>
          </dependency>
  
  
          <dependency>
              <groupId>com.mchange</groupId>
              <artifactId>c3p0</artifactId>
              <version>0.9.5.2</version>
          </dependency>
  
          <dependency> <!--AOP织入包-->
              <groupId>org.aspectj</groupId>
              <artifactId>aspectjweaver</artifactId>
              <version>1.9.4</version>
          </dependency>
  
          <dependency> <!--spring和mybatis整合包-->
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis-spring</artifactId>
              <version>2.0.3</version>
          </dependency>
  
          <dependency> <!--spring和JDBC整合包-->
              <groupId>org.springframework</groupId>
              <artifactId>spring-jdbc</artifactId>
              <version>5.2.2.RELEASE</version>
          </dependency>
  
          <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
              <version>1.18.8</version>
          </dependency>
  
      </dependencies>
  
      <build>
          <resources>
  
              <resource>
                  <directory>src/main/resources</directory>
                  <includes>
                      <include>**/*.properties</include>
                      <include>**/*.xml</include>
                  </includes>
                  <filtering>true</filtering>
              </resource>
  
              <resource>
                  <directory>src/main/java</directory>
                  <includes>
                      <include>**/*.properties</include>
                      <include>**/*.xml</include>
                  </includes>
                  <filtering>true</filtering>
              </resource>
  
          </resources>
      </build>
  
      <properties>
          <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      </properties>
  ```
  

## com.it.dao:

- BookMapper.java：

  ```java
  package com.it.dao;
  
  import com.it.pojo.Books;
  import org.apache.ibatis.annotations.Param;
  
  import java.util.List;
  
  public interface BookMapper {
  
      int addBook(Books book);
  
  
      int deleteBookById(@Param("bookID") int bookID );
  
  
      int updateBook(Books books);
  
  
      Books queryBookById(@Param("bookID")int bookID);
  
      List<Books> queryAllBook();
  
      Books queryBookByName(@Param("bookName") String bookName);
  
  }
  ```

- BookMapper.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.it.dao.BookMapper">
  
  
      <insert id="addBook" parameterType="Books">
          insert into ssmbuild.books(bookName,bookCounts,detail)
          values (#{bookName}, #{bookCounts}, #{detail})
      </insert>
  
  
      <delete id="deleteBookById" parameterType="int">
          delete from ssmbuild.books where bookID=#{bookID}
      </delete>
  
  
      <update id="updateBook" parameterType="Books">
          update ssmbuild.books
          set bookName = #{bookName},bookCounts = #{bookCounts},detail = #{detail}
          where bookID = #{bookID}
      </update>
  
  
      <select id="queryBookById" resultType="Books">
          select * from ssmbuild.books
          where bookID = #{bookID}
      </select>
  
      <select id="queryAllBook" resultType="Books">
          SELECT * from ssmbuild.books
      </select>
  
      <select id="queryBookByName" resultType="Books">
          select * from ssmbuild.books where bookName = #{bookName}
      </select>
  
  </mapper>
  ```

## com.it.pojo：

- Books.java：

  ```java
  package com.it.pojo;
  
  import lombok.AllArgsConstructor;
  import lombok.Data;
  import lombok.NoArgsConstructor;
  
  @Data
  @AllArgsConstructor
  @NoArgsConstructor
  public class Books {
      private int bookID;
      private String bookName;
      private int bookCounts;
      private String detail;
  }
  ```

## com.it.service：

- BookService.java：

  ```java
  package com.it.service;
  
  import com.it.pojo.Books;
  import org.apache.ibatis.annotations.Param;
  
  import java.util.List;
  
  public interface BookService {
  
  
      int addBook(Books book);
  
  
      int deleteBookById(int bookID);
  
  
      int updateBook(Books books);
  
  
      Books queryBookById(int bookID);
  
      List<Books> queryAllBook();
  
      Books queryBookByName(String bookName);
  }
  ```

- BookServiceIml.xml：

  ```xml
  package com.it.service;
  
  import com.it.dao.BookMapper;
  import com.it.pojo.Books;
  
  import java.util.List;
  
  public class BookServiceIml implements BookService {
  
  
      private BookMapper bookMapper;
  
      public void setBookMapper(BookMapper bookMapper) {
          this.bookMapper = bookMapper;
      }
  
  
      public int addBook(Books book) {
          return bookMapper.addBook(book);
      }
  
      public int deleteBookById(int bookID) {
          return bookMapper.deleteBookById(bookID);
      }
  
      public int updateBook(Books books) {
          return bookMapper.updateBook(books);
      }
  
      public Books queryBookById(int bookID) {
          return bookMapper.queryBookById(bookID);
      }
  
      public List<Books> queryAllBook() {
          return bookMapper.queryAllBook();
      }
  
      public Books queryBookByName(String bookName) {
          return bookMapper.queryBookByName(bookName);
      }
  }
  ```

## com.it.comtroller:

- BookController：

  ```java
  package com.it.controller;
  
  import com.it.pojo.Books;
  import com.it.service.BookService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.beans.factory.annotation.Qualifier;
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.PathVariable;
  import org.springframework.web.bind.annotation.RequestMapping;
  
  import java.util.ArrayList;
  import java.util.List;
  
  @Controller
  @RequestMapping("/book")
  public class BookController {
  
      @Autowired
      @Qualifier("BookServiceImpl")
      private BookService bookService;
  
      @RequestMapping("/allBook")
      public String list(Model model) {
          List<Books> list = bookService.queryAllBook();
          model.addAttribute("list", list);
          return "allBook";
      }
  
      @RequestMapping("/toAddBook")
      public String toAddPaper() {
          return "addBook";
      }
  
      @RequestMapping("/addBook")
      public String addPaper(Books books) {
          System.out.println(books);
          bookService.addBook(books);
          return "redirect:/book/allBook";
      }
      //跳转到修改页面
      @RequestMapping("/toUpdateBook")
      public String toUpdateBook(Model model, int id) {
          Books books = bookService.queryBookById(id);
          System.out.println(books);
          model.addAttribute("book",books);
          return "updateBook";
      }
  
      @RequestMapping("/updateBook")
      public String updateBook(Model model, Books book) {
          System.out.println(book);
          bookService.updateBook(book);
          Books books = bookService.queryBookById(book.getBookID());
          model.addAttribute("books", books);
          return "redirect:/book/allBook";
      }
  
      @RequestMapping("/deleteBook/{bookID}")
      public String deleteBook(@PathVariable("bookID") int id){
          bookService.deleteBookById(id);
          return "redirect:/book/allBook";
      }
  
      @RequestMapping("/queryAllBook")
      public String queryBook(String queryBookName,Model model){
          Books books = bookService.queryBookByName(queryBookName);
          List<Books> list = new ArrayList<Books>();
  
          if (books == null){
              list = bookService.queryAllBook();
              model.addAttribute("error","输入为空");
          }
              list.add(books);
              model.addAttribute("list",list);
  
          return "allBook";
      }
  }
  ```

## resources：

- db.properties：

  ```properties
  jdbc.driver=com.mysql.cj.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
  jdbc.username=root
  jdbc.password=3105311
  ```

- mybatis-config.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  
      <settings>
          <setting name="logImpl" value="STDOUT_LOGGING"/>
      </settings>
  
      <typeAliases>
          <typeAlias type="com.it.pojo.Books" alias="Books"/>
      </typeAliases>
  
      <mappers>
          <mapper class="com.it.dao.BookMapper"/>
      </mappers>
  
  </configuration>
  ```

- spring-dao.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  https://www.springframework.org/schema/context/spring-context.xsd">
  
  
      <context:property-placeholder location="classpath:db.properties"/>
  
  
      <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
  
          <property name="driverClass" value="${jdbc.driver}"/>
          <property name="jdbcUrl" value="${jdbc.url}"/>
          <property name="user" value="${jdbc.username}"/>
          <property name="password" value="${jdbc.password}"/>
  
  
  <!--        <property name="maxPoolSize" value="30"/>-->
  <!--        <property name="minPoolSize" value="10"/>-->
  
  <!--        <property name="autoCommitOnClose" value="false"/>-->
  
  <!--        <property name="checkoutTimeout" value="10000"/>-->
  
  <!--        <property name="acquireRetryAttempts" value="2"/>-->
      </bean>
  
  
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  
          <property name="dataSource" ref="dataSource"/>
  
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
      </bean>
  
  
      <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  
          <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
  
          <property name="basePackage" value="com.it.dao"/>
      </bean>
  
  </beans>
  ```

- spring-service.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd">
  
      <!-- 扫描service相关的bean -->
      <context:component-scan base-package="com.it.service" />
  
      <!--BookServiceImpl注入到IOC容器中-->
      <bean id="BookServiceImpl" class="com.it.service.BookServiceIml">
          <property name="bookMapper" ref="bookMapper"/>
      </bean>
  
      <!-- 配置事务管理器 -->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <!-- 注入数据库连接池 -->
          <property name="dataSource" ref="dataSource" />
      </bean>
  
  </beans>
  ```

- spring-mvc.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/mvc
      https://www.springframework.org/schema/mvc/spring-mvc.xsd">
  
      <mvc:annotation-driven />
  
      <mvc:default-servlet-handler/>
  
  
      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
          <property name="prefix" value="/WEB-INF/jsp/"/>
          <property name="suffix" value=".jsp" />
      </bean>
  
  
      <context:component-scan base-package="com.it.controller"/>
  
  </beans>
  ```

- applicationContext.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  
      <import resource="classpath:spring-dao.xml"/>
      <import resource="classpath:spring-service.xml"/>
      <import resource="classpath:spring-mvc.xml"/>
  
  </beans>
  ```

## WEB-INF:

- web.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
           version="4.0">
  
      <!--DispatcherServlet-->
      <servlet>
          <servlet-name>DispatcherServlet</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:applicationContext.xml</param-value>
          </init-param>
          <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
          <servlet-name>DispatcherServlet</servlet-name>
          <url-pattern>/</url-pattern>
      </servlet-mapping>
  
      <!-- spring编码过滤器-->
      <filter>
          <filter-name>encodingFilter</filter-name>
          <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
          <init-param>
              <param-name>encoding</param-name>
              <param-value>UTF-8</param-value>
          </init-param>
          <init-param>
              <param-name>forceEncoding</param-name>
              <param-value>true</param-value>
          </init-param>
      </filter>
  
      <filter-mapping>
          <filter-name>encodingFilter</filter-name>
          <url-pattern>/*</url-pattern>
      </filter-mapping>
  
      <!--Session过期时间-->
      <session-config>
          <session-timeout>15</session-timeout>
      </session-config>
  
  </web-app>
  ```

- index.jsp：

  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
  <!DOCTYPE HTML>
  <html>
  <head>
    <title>首页</title>
    <style type="text/css">
      a {
        text-decoration: none;
        color: black;
        font-size: 18px;
      }
      h3 {
        width: 180px;
        height: 38px;
        margin: 100px auto;
        text-align: center;
        line-height: 38px;
        background: deepskyblue;
        border-radius: 4px;
      }
    </style>
  </head>
  <body>
  
  <h3>
    <a href="${pageContext.request.contextPath}/book/allBook">点击进入列表页</a>
  </h3>
  </body>
  </html>
  ```

  

## WEB-INF.jsp:

- addBook.jsp：

  ```jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  
  <html>
  <head>
      <title>增加书籍</title>
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <!-- 引入 Bootstrap -->
      <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body>
  <div class="container">
  
      <div class="row clearfix">
          <div class="col-md-12 column">
              <div class="page-header">
                  <h1>
                      <small>增加书籍</small>
                  </h1>
              </div>
          </div>
      </div>
      <form action="${pageContext.request.contextPath}/book/addBook" method="post">
          书籍名称：<input type="text" name="bookName"><br><br><br>
          书籍数量：<input type="text" name="bookCounts"><br><br><br>
          书籍详情：<input type="text" name="detail"><br><br><br>
          <input type="submit" value="添加">
      </form>
  
  </div>
  ```

- allBook.jsp：

  ```jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>书籍列表</title>
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <!-- 引入 Bootstrap -->
      <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body>
  
  <div class="container">
  
      <div class="row clearfix">
          <div class="col-md-12 column">
              <div class="page-header">
                  <h1>
                      <small>书籍列表 —— 显示所有书籍</small>
                  </h1>
              </div>
          </div>
      </div>
                                  <%--class="form-inline"变为行内元素，不独占一行--%>
      <div class="row">
          <div class="col-md-4 column">
              <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
              <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/allBook">显示全部书籍</a>
          </div>
          <div class="col-md-4 column"></div>
          <div class="col-md-4 column">
              <span class="form-inline" style="color: red;font-weight: bold;">${error}</span>
              <form class="form-inline" action="${pageContext.request.contextPath}/book/queryAllBook" method="get" style="flow:right">
                  <input type="text" name="queryBookName" class="form-control" placeholder="请输入要查询的书籍名称">
                  <input type="submit" value="查询" class="btn btn-primary">
              </form>
          </div>
      </div>
  
      <div class="row clearfix">
          <div class="col-md-12 column">
              <table class="table table-hover table-striped">
                  <thead>
                  <tr>
                      <th>书籍编号</th>
                      <th>书籍名字</th>
                      <th>书籍数量</th>
                      <th>书籍详情</th>
                      <th>操作</th>
                  </tr>
                  </thead>
  
                  <tbody>
                  <c:forEach var="book" items="${requestScope.get('list')}">
                      <tr>
                          <td>${book.getBookID()}</td>
                          <td>${book.getBookName()}</td>
                          <td>${book.getBookCounts()}</td>
                          <td>${book.getDetail()}</td>
                          <td>
                              <a href="${pageContext.request.contextPath}/book/toUpdateBook?id=${book.getBookID()}">更改</a> |
                              <a href="${pageContext.request.contextPath}/book/deleteBook/${book.getBookID()}">删除</a>
                          </td>
                      </tr>
                  </c:forEach>
                  </tbody>
              </table>
          </div>
      </div>
  </div>
  ```

- updateBook.jsp：

  ```jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>修改信息</title>
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <!-- 引入 Bootstrap -->
      <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body>
  <div class="container">
  
      <div class="row clearfix">
          <div class="col-md-12 column">
              <div class="page-header">
                  <h1>
                      <small>修改信息</small>
                  </h1>
              </div>
          </div>
      </div>
  
      <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
          <input type="hidden" name="bookID" value="${book.getBookID()}"/>
          书籍名称：<input type="text" name="bookName" value="${book.getBookName()}"/>
          书籍数量：<input type="text" name="bookCounts" value="${book.getBookCounts()}"/>
          书籍详情：<input type="text" name="detail" value="${book.getDetail() }"/>
          <input type="submit" value="提交"/>
      </form>
  
  </div>
  ```

  

