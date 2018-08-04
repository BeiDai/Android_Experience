# Java服务端编程(HTTP+Tomcat+MyEclipse+Servlet)

## 第一讲

- JSP基础及Tomacat服务器的配置
- 深入Http协议

      Http协议详解
      Http协议请求消息与应答消息（get，post）抓包详解
      Http中的GET与POST的本质区别

- Java服务端Servlet编程

      eclipse中配置j2ee开发环境以及tomacat服务器
      Myeclipse中配置Tomcata开发环境

### 准备开发工具

     MyEclipse.exe  apache-tomcat.exe

### 学习内容

- 掌握B/S开发的基本概念    
  - 动态网页pk静态网页
  - B/S程序pk C/S程序
  - B/S开发涉及的技术内容

- 开发JSP动态网站的基本步骤
  - 掌握Web系统的工作目录
  - 创建Web工程与HTML、JavaScript文件
  - 创建Web工程的部署与运行
  - 进行Web系统的调试与排错

#### 动态网页pk静态网页

    动态网站：客户端与服务端数据交互(维护简单)
    静态网站：相当于公告 HTML格式(速度快)
    **浏览器只识别HTML**
    Web服务器：服务端语言(php,jsp) -> HTML
    静态页技术：提前把网页语言转化HTML(更新频率不高)

#### B/S程序 pk C/S程序

    Android程序：C/S程序   客户端与服务器(中心局域网)

    B/S程序：用户(浏览器+网址) - 应用服务器 - 数据库服务器

    应用程序在客户端 C/S : 速度快、性能好、功能强大(本地文件)

    应用程序在服务端 B/S : 维护简单、数据安全、小程序

## 开发JSP动态网站的基本步骤

1、创建一个Web项目

    菜单栏：文件 新建  项目

    窗口中选择：Web Project

    输入项目名称与其他信息

2、设计Web项目的目录结构

    src文件夹：存放Java源文件

    WebRoot：Web应用根目录

    META-INF：存放系统描述信息

    WEB-INF：内容不能对外发布

    lib文件夹: 存放以jar/zip形式库文件

    web.xml: Web应用初始化配置文件

3、编写Web项目的代码

4、部署Web项目

5、运行Web项目

    URL ：统一资源定位系统

    http://localhost:8080/news/index.html

    协议 + 主机IP地址/端口号 + 主机资源具体地址

## Http协议与J2EE技术

- 了解JavaEE的体系结构
- 了解JavaEE的技术内容
- 了解JavaEE的分层结构
- 理解并掌握HTTP请求和响应

    JavaEE技术：开发分布式企业级应用的规范和标准

    Struts  Hibernate  Spring 他们都是框架，某种应用的半成品

    平房服务机构 ： Jsp + Servlet + DAO + MVC + Android

    JavaEE：分层 一层：服务大厅 二层：办公室  三层：资料室
    表示层 ---- HTML界面、JavaScript、Ajax技术
    中间层 ---- Servlet等组件、JSP、JavaBean
    数据层 ---- Database(JDBC JNDI等)

### HTTP协议的定义

- 超文本传输协议
  - 无状态协议
    - 不用记录谁发出请求
    - 适用于传输文件
  - 用于通过Internet发送请求和响应消息
  - 使用端口接收和发送消息，默认80端口

解决无状态问题：Session、Cookie、Application

- TCP/IP Monitor 监听工具

      Preference 配置

      Add  8088  localhost 8080

### HTTP 协议响应码

在接收和解释请求消息后，服务器返回一个HTTP响应消息

HTTP响应消息由三部分组成：状态行、消息报头、响应正文

1、状态行格式如下：

      HTTP-Version Status-Code Reason-Phrase CRLF

      HTTP-Version ：表示服务器HTTP协议版本

      Status-Code ：服务器发回的响应状态码

      eason-Phrase ：状态码的文本描述

      状态码由三位数字组成，第一个数字定义的响应类别，且有五种可能：

      1xx: 指示信息 -- 表示请求已接收，继续处理
      2xx: 成功 -- 表示请求已被成功接收、处理、接受
      3xx：重定向 -- 要完成请求必须进行更进一步的操作
      4xx：客户端错误 -- 请求有语法错误或请求无法实现
      5xx：服务端错误 -- 服务器未能实现合法的请求
      常见状态码、状态描述、说明：
      200 OK  // 表示成功
      400 Bad Request // 客户端请求有语法错误，不能被服务器理解
      401 Unauthorized // 请求未经授权，这个状态码必须和WWW-Authenticate头域一起使用
      403 Forbidden  // 服务器收到请求，但拒绝提供服务
      404 Not Found  // 请求资源不存在 eg.输入了错误的URL
      500 Internal Server Error // 服务器发生了不可预期的错误
      503 Server Unavaliable // 服务器当前不能处理客户端的请求，一段时间后，可恢复正常
      eg. HTTP/1.1 200 OK (CRLF)

      2、响应报头后述

      3、响应正文就是服务器返回的资源的内容

### HTTP协议消息报头

HTTP消息由客户端到服务器的请求和服务器到客户端的响应组成。请求消息和响应消息都是由开始行（对于请求消息，开始行就是请求行，对于响应消息，开始行就是状态行），消息报头（可选），空行（只有CRLF的行），消息正文（可选）组成。

HTTP消息报头包括普通报头、请求报头、响应报头、实体报头。

每一个报头域都是由    
    名字+“：”+空格+值     
组成，消息报头域的名字与大小写无关    

**普通报头** : 在普通报头中，有少数报头域用于所有的请求和响应消息，但并不用于被传输的实体，只用于传输消息。

**请求报头** ： 请求报头允许客户端向服务器端传递请求的附加信息以及客户端自身的信息。

**响应报头** ： 响应报头允许服务器传递不能放在状态行中的附加响应消息，以及关于服务器的信息的Request-URL所标识资源进行下一步访问的信息

**实体报头** ： 请求和响应消息都可以传送一个实体，一个实体有实体报头域和实体正文组成，但并不是说实体报头域和实体正文要在一起发送，可以只发送实体报头域。实体报头定义了关于实体正文（eg: 有无实体正文)和请求所标识的资源的元信息。

## Servlet

    1、理解Servlet的生命周期
    2、会使用Servlet处理GET、POST请求
    3、会使用Servlet处理页面的转向
    4、会配置web.xml文件

Servlet：是一个服务端的Java程序，在服务器上运行以处理客户端请求并做出响应的程序

    import java.io.*;
    import javax.servlet.*;
    import javax.servlet.http.*;
    public class HelloServlet extends HttpServlet{
      public void doGet(HttpServletRequest request,HttpServletResponse reponse) throws ServletException,IOException{
        response.setContentType("text/html;charset=gb2312");
        PrintWriter out response.getWriter();
        out.println("<html>");
        out.println(" <head><title>Servlet</title></head>");
        out.println(" <body>");
        out.println("你好，欢迎来到Servlet世界")；
        out.println(" </body>");
        out.println("</html>");
        out.close();
      }
    }


   **Servlet与JSP关系**

    把JSP代码转化为Servlet输入输出


## Servlet代码编写


    // 新建servlet类，可以把 doGet(request,response);改到doPost
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        doGet(request,response);

    }


    // 在web.xml中可已更改目录路径
    // 原始：http://localhost:8080/HelloJ2EE/servlet/MyServlet
    // 现在：http://localhost:8080/HelloJ2EE/MyServlet
    <servlet-mapping>
      <servlet-name>MyServlet</servlet-name>
      <url-pattern>/MyServlet</url-pattern>
      <!-- <url-pattern>/servlet/MyServlet</url-pattern> -->
    </servlet-mapping>

    // 改 后缀
    <url-pattern>/MyServlet.do</url-pattern>
    // 现在：http://localhost:8080/HelloJ2EE/MyServlet.do



## 理解Servlet的生命周期

- 实例化
  - Servlet容器创建Servlet的实例
- 初始化
  - 该容器调用init()方法
- 服务
  - 如果请求Servlet,则容器调用service()方法
- 销毁
  - 销毁实例之前调用destroy()方法

- Servlet中文乱码问题

      response.setContentType("text/html,charset=GBK")

## Servlet部署
