

# JavaWeb

## 1、基本概念

### 1.1、前言

web开发：

- web，网页的意思
- 静态web
  - html，css
  - 提供给所有人看的数据始终不会发生变化
- 动态web
  - 几乎所有的网站
  - 提供给所有人看的数据始终会发生变化
  - 技术栈：Servlet/JSP，ASP，PHP

在Java中，动态web资源开发的技术统称为JavaWeb

### 1.2、Web应用程序

web应用程序：可以提供浏览器访问的程序

- a.html、b.html....多个web资源，这些资源可以被外界访问，对外界提供服务
- 能访问到的任何一个资源，都存在在一个计算机上
- URL
- 这些统一的web资源会被放在同一个文件夹下。web引用程序->tomcat服务器
- 一个web应用有多部分组成（静态web，动态web）
  - html，css，js
  - jsp，servlet
  - Java程序
  - jar包
  - 配置程序（properties）

web引用程序编写完毕后，想要提供给外界访问：需要一个服务器来统一管理

### 1.3、静态web

- *.htm,@.html，这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取

![image-20200430152552562](../../AppData/Roaming/Typora/typora-user-images/image-20200430152552562.png)

- 静态web的缺点
  - Web页面无法动态跟新，所有用户看到的都是同一个页面
    - 轮播图，点击特效：伪动态
    - JavaScript【实际开发中，用的最大】
    - VBScript
  - 无法和数据库交互（数据无法持久化，用户无法互动）

### 1.4、动态Web

页面会动态展示：web页面的展示因人而异

![image-20200430153132431](../../AppData/Roaming/Typora/typora-user-images/image-20200430153132431.png)

缺点：

- 如果服务器的动态web资源出现了错误，需要重写编写**后台程序**
  - 停机维护

优点

- Web页面可以动态跟新，所有用户看到的不是同一个页面
- 可以和数据库交互（使用JDBC）

## 2、Web服务器

### 2.1、技术讲解

JSP/Servlet：

B/S：浏览和服务器

C/S：客户端和服务器

- sun公司主推的B/S架构
- 基于Java语言（所有的大公司，或者一些开源的组件，都是用Java写的）
- 可以承载三高（高并发，高可以，高性能）问题带来的影响

### 2.2、Web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息

Tomcat：

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，Tomcat 5支持最新的Servlet 2.4 和JSP 2.0 规范。因为Tomcat 技术先进、性能稳定，而且**免费**，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/服务器)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个初学者来说，它是最佳的选择。

Tomcat 实际上运行JSP 页面和Servlet。目前Tomcat最新版本为**10.0.0-M3。**

## 3、Tomcat

### 3.1、文件夹作用

![image-20200429222047081](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200429222047081.png)



### 3.2、配置

#### 3.2.1、服务器核心配置

![image-20200429222505238](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200429222505238.png)

#### 3.2.2、可以启动的端口号：

- tomcat默认端口号：8080
- mysql：3306
- http：80
- https：443

#### 3.2.3、可以配置主机的名称

- 默认的主机名为：localhost->127.0.0.1
- 默认的网站引用存放的位置为：webapps

```xml
<Host name="localhost" autoDeploy="true" unpackWARs="true" appBase="webapps">
```

### 3.3、高难度面试题：

#### 请你谈谈网站是如何进行访问的！

1. 输入一个域名；回车

2. 检测本机的C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名的映射

   1. 有：直接返回对应的ip地址

      ```
      127.0.0.1       localhost
      ```

   2. 没有：去DNS服务器上找，找到就返回，找不到就返回找不到

![image-20200430160316760](../../AppData/Roaming/Typora/typora-user-images/image-20200430160316760.png)

### 3.4、发布一个web网站

- 将自己写的网站，放到服务器（Tomcat）中指定的web应用的文件夹（webapps）下

## 4、Http

### 4.1、什么是Http

Http（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上

### 4.2、两个时代

- http1.0
  - HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，断开连接

- http2.0
  - HTTP/1.1：客户端可以与web服务器连接后，可以获得多个web资源。

### 4.3、HTTP请求

- 客户端---发请求---服务器z

```java
Request URL: https://www.baidu.com/		请求地址
Request Method: GET						get方法
Status Code: 200 OK						状态码
Remote（远程） Address: 14.215.177.39:443		真实地址		
```

```java
Accept: text/html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: no-cache
Connection: keep-alive
```

#### 1、请求行

- 请求行中的请求方式：GET
- 请求方法：Get/Post，
  - get：请求携带的参数比较少，大小由限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
  - post：请求能够携带的参数没有限制，大小没有限制，不会再浏览器的URL地址栏显示数据内容，安全，但不高效

#### 2、消息头

```
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：浏览器支持的编码格式
Accept-Language：告诉浏览器，它的的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机
```



### 4.4、Http响应

- 服务器---响应---客户端

```java
Cache-Control: private					缓存控制
Connection: Keep-Alive					连接
Content-Encoding: gzip					编码
Content-Type: text/html;charset=utf-8	类型
```

#### 1、响应体

```java
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：浏览器支持的编码格式
Accept-Language：告诉浏览器，它的的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机
Refresh：告诉客户端，多久刷新一次
Location：让网页重新定位
```

#### 2、响应状态码

200：请求响应成功

3xx：请求重定向

- 重定向：跳转到一个新位置

4xx：找不到资源     404

5xx：服务器代码错误	500	502：网关错误



### 常见面试题：

#### 当你的浏览器地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么？

Servlet是由web服务器调用，web服务器在收到浏览器请求之后，会

1.浏览器向服务器发出GET请求(请求服务器ServletA)

2.服务器上的容器逻辑接收到该url,根据该url判断为Servlet请求，此时容器逻辑将产生两个对象：请求对象(HttpServletRequest)和响应对象(HttpServletResponce)

3.容器逻辑根据url找到目标Servlet(本示例目标Servlet为ServletA),且创建一个线程A

4.容器逻辑将刚才创建的请求对象和响应对象传递给线程A

5.容器逻辑调用Servlet的service()方法

6.service()方法根据请求类型(本示例为GET请求)调用doGet()(本示例调用doGet())或doPost()方法

7.doGet()执行完后，将结果返回给容器逻辑

8.线程A被销毁或被放在线程池中





## 5、Servlet

### 5.1、Servlet简介

- Servlet就是sun公司开发动态web的一门技术
- sun在这些API中提供一个接口叫做：Servlet，开发一个Servlet接口，只需要：
  - 编写一个类，实现Servlet接口
  - 把Java类部署到web服务器中

把实现了Servlet接口的程序叫做Servlet

### 5.2、HttpServlet

Servlet接口sun公司有两个默认的实现类：HttpServlet，GenericServlet

![image-20200430181423293](../../AppData/Roaming/Typora/typora-user-images/image-20200430181423293.png)



1. 构建一个普通的Maven项目，删掉里面的src目录，让在在这个项目中创建Module；这个空的Maven工程就是Maven主工程

2. 关于Maven父子工程的理解：

   父项目中会有：

   ```xml
   <modules>
       <module>sevlet-01</module>
   </modules>
   ```

   子项目中会有：

   ```xml
   <parent>
       <artifactId>javaweb_01</artifactId>
       <groupId>com.hc</groupId>
       <version>1.0-SNAPSHOT</version>
   </parent>
   ```

   父项目的Java子项目可以直接使用

   ```java
   son extends father
   ```

3. 编写一个Servlet程序

   1. 编写一个普通类

   2. 实现Servlet接口，这里我们直接继承HttpServlet

      ```java
      public class HelloServlet extends HttpServlet {
      
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              //req.getInputStream();
              //ServletOutputStream outputStream = resp.getOutputStream();
              PrintWriter writer = resp.getWriter();//响应流
              writer.print("Hello Servlet");
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              doGet(req, resp);
          }
      }
      ```

4. 编写Servlet的映射
   
   - 为什么需要映射：我们写的是Java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务器中注册我们写的Servlet，还需要给它一个浏览器能够访问的路径

### 5.3、Servlet原理

Servlet是由web服务器调用，web服务器在收到浏览器请求之后，会

1.浏览器向服务器发出GET请求(请求服务器ServletA)

2.服务器上的容器逻辑接收到该url,根据该url判断为Servlet请求，此时容器逻辑将产生两个对象：请求对象(HttpServletRequest)和响应对象(HttpServletResponce)

3.容器逻辑根据url找到目标Servlet(本示例目标Servlet为ServletA),且创建一个线程A

4.容器逻辑将刚才创建的请求对象和响应对象传递给线程A

5.容器逻辑调用Servlet的service()方法

6.service()方法根据请求类型(本示例为GET请求)调用doGet()(本示例调用doGet())或doPost()方法

7.doGet()执行完后，将结果返回给容器逻辑

8.线程A被销毁或被放在线程池中

![](../../AppData/Roaming/Typora/typora-user-images/image-20200430220257446.png)

### 5.4、Mapping问题

1. 一个Servlet可以指定一个映射路径

   ```xml
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   ```

2. 一个Servlet可以指定多个映射路径

   ```xml
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/hello2</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/hello3</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/hello4</url-pattern>
   </servlet-mapping>
   ```

3. 一个Servlet可以指定通用映射路径

   ```xml
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/hello/*</url-pattern>
   </servlet-mapping>
   ```

4. 默认请求路径

   ```xml
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

5. 指定一些后缀或者前缀等等

   ```xml
   *前面不能加映射的路径
   但是所有以.hc结尾的路径都会跳转到这个地址
   <servlet-mapping>
       <servlet-name>helloServlet</servlet-name>
       <url-pattern>*.hc</url-pattern>
   </servlet-mapping>
   ```

6. 优先级问题

   指定了固有的映射路径优先级更高，如果找不到就会走默认的处理请求

   ```xml
   <servlet>
       <servlet-name>errorServlet</servlet-name>
       <servlet-class>com.hc.servlet.ErrorServlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>errorServlet</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

### 5.5、ServletContext对象

web容器在启动的时候，容器会创建一个ServletContext(上下文),这个WEB项目所有部分都将共享这个上下文.

#### 5.5.1、共享数据-

- 在这个Servlet总保存的数据，可以在另外一个Servlet中拿到

存放：

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //this.getInitParameter()   初始化参数
    //this.getServletConfig()   Servlet配置
    //this.getServletContext()  Servlet上下文
    ServletContext servletContext = this.getServletContext();
    String username = "韩成";
    servletContext.setAttribute("username",username);//将一个数据保存在ServletContext中

    System.out.println("Hello");
}

@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
}
```

获取并输出：

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    resp.setContentType("text/html");
    resp.setCharacterEncoding("utf-8");

    ServletContext servletContext = this.getServletContext();
    String username = (String) servletContext.getAttribute("username");

    resp.getWriter().print("名字:"+username);
}

@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
}
```

```xml
<servlet>
    <servlet-name>helloServlet</servlet-name>
    <servlet-class>com.hc.servlet.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>helloServlet</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>getServlet</servlet-name>
    <servlet-class>com.hc.servlet.GetServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>getServlet</servlet-name>
    <url-pattern>/get</url-pattern>
</servlet-mapping>
```

#### 5.5.2、获取初始化参数

存储：

```xml
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
```

获取：

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext servletContext = this.getServletContext();
    String url = servletContext.getInitParameter("url");
    resp.getWriter().print(url);
}
```

#### 5.5.3、请求转发

请求转发是一次访问，并且路径不变

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
//        RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/servletContext");
//        requestDispatcher.forward(req,resp);
        servletContext.getRequestDispatcher("/servletContext").forward(req,resp);
    }
```

#### 5.5.4、读取资源文件

1、在resource目录下创建一个properies文件

2、创建一个类继承HttpServlet类并读取信息

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    InputStream resourceAsStream = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");

    Properties properties = new Properties();
    properties.load(resourceAsStream);
    String user = properties.getProperty("username");
    String pass = properties.getProperty("password");

    resp.getWriter().print(user+":"+pass);
}
```

### 5.6、HttpServletResponse

web服务器接收到客户端的Http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，和一个代表响应的HttpServletResponse对象；

- 如果要获取客户端请求过来的参数：找HttpServletRequest
- 如果要给客户端响应一些信息：找HttpServletResponse

#### 5.6.1、简单分类

##### 5.6.1.1、负责向浏览器发送数据的方法

- ```java
  public ServletOutputStream getOutputStream() throws IOException;
  public PrintWriter getWriter() throws IOException;
  ```

##### 5.6.1.2、负责向浏览器发送响应头的方法

- ```java
  HttpServletResponse
  public void setCharacterEncoding();
  public void setContentType();
  public void setContentLength(int len);
  public void setContentType(String type);
  
  HttpServlet
  public void setDateHeader(String name, long date);
  public void addDateHeader(String name, long date);
  public void setHeader(String name, String value);
  public void addHeader(String name, String value);
  public void setIntHeader(String name, int value);
  public void addIntHeader(String name, int value);
  ```

#### 5.6.2、常见应用

1.向浏览器输出信息

```java
resp.getWriter.pringt();
```



##### 5.6.2.1、下载文件

1. 获取下载文件的路径
2. 获取下载的文件名
3. 让浏览器支持需要下载的东西
4. 获取下载问的输入流
5. 创建缓冲区
6. 获取OutputStream对象
7. 将FileOutputStream流写入到buffer缓冲区
8. 使用OutputStream将缓冲区中的数据输入到客户端

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //1. 获取下载文件的路径
    //String realPath = this.getServletContext().getRealPath("/1.jpg");
    String realPath = "E:\\idea目录\\javaweb\\javaweb_01\\respons\\src\\main\\resource\\1.jpg";
    //2. 获取下载的文件名
    String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
    System.out.println("要下载文件的路径:"+fileName);
    //3. 让浏览器支持需要下载(Content-Disposition)的东西,中文文件名URLEncoder.encode编码，否则有可能乱码
    resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(fileName,"utf-8"));
    //4. 获取下载问的输入流
    FileInputStream fileInputStream = new FileInputStream(realPath);
    //5. 创建缓冲区
    int len = 0;
    byte[] bytes = new byte[1024];
    //6. 获取OutputStream对象
    ServletOutputStream outputStream = resp.getOutputStream();
    //7. 将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输入到客户端
    while((len=fileInputStream.read(bytes)) > 0 ){
        outputStream.write(bytes,0,len);
    }
    outputStream.close();
    fileInputStream.close();
}
```

##### 5.6.2.2、验证码功能

验证怎么来的？

- 前端实现
- 后端实现，需要用到Java图片类，生产一个图片

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //如何让浏览器5秒刷新一次
    resp.setHeader("refresh","3");

    //在内存中创建一个图片
    BufferedImage image = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
    //得到图片
    Graphics2D graphics = (Graphics2D) image.getGraphics();//笔
    //设置图片的背景颜色
    graphics.setColor(Color.WHITE);
    graphics.fillRect(0,0,80,20);
    //给图片写数据
    graphics.setColor(Color.BLUE);
    graphics.setFont(new Font(null,Font.BOLD,20));
    graphics.drawString(makeNum(),0,20);

    //告诉浏览器这个请求用图片的方式打开
    resp.setContentType("image/jpeg");
    //网站存在缓存，不然浏览器缓存
    resp.setDateHeader("expires",-1);
    resp.setHeader("Cache-Control","no-cache");
    resp.setHeader("Pragma","no-cache");

    //把图片写给浏览器
    ImageIO.write(image,"jpg", resp.getOutputStream());
}

//生成随机数
private String makeNum(){
    Random random = new Random();
    String num = random.nextInt(9999999)+"";
    StringBuffer stringBuffer = new StringBuffer();
    for(int i = 0; i < 7-num.length(); i++){
        stringBuffer.append("0");
    }
    num = stringBuffer.toString()+num;
    return num;
}
```

##### 5.6.2.3、实现重定向

一个web资源收到客户端请求后，他会通知客户端去访问另外一个web资源，这个过程叫重定向

常见场景：

- 用户登录

```java
void sendRedirect(String var1) throws IOException
```

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //resp.setHeader("Location","/image");
    //resp.setStatus(302);
    resp.sendRedirect("/image");
}
```

相同点：

- 页面都会实现跳转

不同点：

- 请求转发的时候，url不会发生变化		307
- 重定向的时候，url地址栏会发生变化     302
- 请求转发是一起访问

### 5.7、HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，Http请求中的信息会被封装到HttpServletRequest，通过这个HttpServletRequest方法，获得客户端的所有信息

#### 5.7.1、常见应用

##### 5.7.1.1、获取参数,请求转发(请求转发是内部资源调用)

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");

        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbys = req.getParameterValues("hobbys");
        System.out.println("=============================");
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbys));
        System.out.println("=============================");

        //请求转发
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }
```

相同点：

- 页面都会实现跳转

不同点：

- 请求转发的时候，url不会发生变化		307
- 重定向的时候，url地址栏会发生变化     302
- 请求转发是一起访问

##### 5.7.1.2、请求转发：请求转发是内部资源调用





## 6、Cookie、Session

### 6.1、会话

**会话**：用户打开一个浏览器，点击超链接，访问web资源，关闭浏览器，这个过程叫会话

**有状态会话**：服务器会记录你的访问信息，下次访问时还会根据信息匹配到你



### 6.2、保存会话的两种技术

cookie：

- 客户端技术（响应，请求）

session：

- 服务器技术，利用这个技术，可以保存用户的会话信息。把信息保存到Session中



### 6.3、Cookie

1. 从请求中拿到Cookie信息
2. 服务器响应给客户端Cookie

```java
 	@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //保存进入服务器的时间，下次来的时候带进来
        req.setCharacterEncoding("utf-8");
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");

        PrintWriter out = resp.getWriter();
        //Cookie,服务器端从客户端获取
        Cookie[] cookies = req.getCookies();//返回数组，说明Cookie可能存在多个

        //判断Cookie是否存在
        if(cookies!=null){
            //如果存在
            out.write("上一次访问的时间是：");
            for (int i = 0; i < cookies.length; i++){
                Cookie cookie = cookies[i];
                //获取Cookie的名字
                if(cookie.getName().equals("Date")){
                    //获取Cookie中的值
                    Long dateLong = Long.parseLong(cookie.getValue());
                    Date date = new Date(dateLong);
                    out.write(date.toLocaleString());
                }
            }
        }else{
            out.write("这是您第一次访问本站");
        }
```



Cookie：一般会保存在本地的用户目录下



#### 细节问题：

- 一个Cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个cookie，上限是20个
- Cookie大小由限制：4kb
- 浏览器保存cookie的上限是300个

删除Cookie：

- 不设置有效期，关闭浏览器，自动失效
- 设置有效期为0



#### 编码解码：

```java
//解码
URLDecoder.decode(cookie.getValue(),"utf-8")
//编码
URLEncoder.encode("韩成","utf-8")
```



### 6.4、Session（重点）

#### 什么是Session

- 服务器会给每一个用户（浏览器）创建一个Session对象
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就一直存在
- 用户登录后，整个网站它都可以访问-->保存用户的信息



![image-20200502213822977](../../AppData/Roaming/Typora/typora-user-images/image-20200502213822977.png)

#### Session和Cookie的区别：

- Cookie是把用户的数据写在浏览器，会话结束即失效（**只要设置了过期时间cookie就会存储在硬盘里面**）
- Session是把用户的数据写到用户独占的Session中，服务器端保存（保存重要的信息，嫌少服务器的负担）
- Session对象由服务器创建



使用场景：

- 保存一个登录用户的信息
- 购物车信息
- 在整个网站中经常使用的数据，我们将他保存在Sesion中



#### 使用Session

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //解决乱码问题
    req.setCharacterEncoding("utf-8");
    resp.setContentType("text/html");
    resp.setCharacterEncoding("utf-8");

    //得到Session
    HttpSession session = req.getSession();

    //给Session中存东西
    session.setAttribute("name",new Person("韩成",23));

    //获取Session的ID
    String sessionId = session.getId();

    //判断Session是不是新创建的
    if (session.isNew()){
        resp.getWriter().write("Session创建成功，ID："+sessionId);
    }else{
        resp.getWriter().write("Session在服务器已经存在了，ID："+sessionId);
    }

}
```





#### 会话的两种过期方式：

##### 自动销毁

- web.xml

```xml
<!--设置Session默认的失效时间-->
<session-config>
  <!--15分分钟后Session自动失效-->
  <session-timeout>15</session-timeout>
</session-config>
```

- 代码配置

```java
//以秒为单位
session.setMaxInactiveInterval(60);
```

##### 手动销毁

```java
getSession().invalidate();
```



## 7、JSP

### 7.1、什么是JSP

Java Server Pages：Java服务器端页面，也和Servlet一样，用于动态web技术

最大的特定：

- 写JSP就想写html
- 区别：
  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入Java代码，为用户提供动态数据



### 7.2、JSP原理

思路：JSP到底怎么执行的！

- 服务器内部工作

  tomcat中有一个work目录

  IDEA中使用tomcat会在IDEA的tomcat目录中生产一个work目录



**浏览器想服务器发送请求，不管访问什么资源，其实都是在访问servlet！**

JSP最终会被转换成一个Java类！

**JSP本质上就是一个Servlet**

```java
//初始化
public void _jspInit(){
}

//销毁
public void _jspDestroy(){
}

//JSPService
public void _jspService(HttpServletRequest req,HttpServletResponse resp){

}
```



1、判断请求

#### 2、内置一些对象

```java
final javax.servlet.jsp.PageContext pageContext;	//页面上下文
javax.servlet.http.HttpSession session = null;		//session
final javax.servlet.ServletContext application;		//applicationContext
final javax.servlet.ServletConfig config;			//config
javax.servlet.jsp.JspWriter out = null;				//out
final java.lang.Object page = this;					//page：当前页
HttpServletRequest req								//请求
HttpServletResponse resp							//响应
```

#### 3、输出页面前增加的代码

```java
response.setContentType("text/html");			//设置响应的页面类型
pageContext = _jspxFactory.getPageContext(this, request, response,
        null, true, 8192, true);
_jspx_page_context = pageContext;
application = pageContext.getServletContext();
config = pageContext.getServletConfig();
session = pageContext.getSession();
out = pageContext.getOut();
_jspx_out = out;
```

4、以上的这些对象我们可以在JSP页面中直接使用！



在JSP页面中：

- 只要是Java代码就会原封不动的输出；

- 如果是HTML代码，就会被转换为

  ```java
  out.write("<html>\r\n");
  ```

  这样的格式，输出到前端



### 7.3、JSP基础语法

任何语言都有自己的语法，Java也有，JSP作为Java技术的一直应用，它拥有一些自己扩充的语法（了解，知道即可），Java所有语法都支持！



#### JSP表达式<%=%>

```jsp
<%--JSP表达式
作用：用来将程序的输出，输出到客户端
<%= 变量名或者表达式%>
--%>
//(= new java.util.Date())=out.print(new java.util.Date())
<%= new java.util.Date() %>
```



#### JSP脚本片段

```jsp
<%--JSP脚本片段--%>
<%
    int sum = 0;
    for (int i = 0; i < 100; i++){
        sum+=i;
    }
    out.println("<h1>Sum="+sum+"</h1>");
%>
```



#### 脚本片段的在实现<%%>

```jsp
<%
    for(int i = 0; i < 5; i++){
%>
<h1>Hello World<%=i%></h1>
<%
    }
%>
```



#### JSP声明<%!%>

**JSP声明会被编译到生成的Java的类中，脚本片段只会在_jspService()方法中**

```jsp
<%!
   static {
       System.out.println("Loading Servlet!");
   }

   private int globalVar = 0;

   public void hc(){
       System.out.println("进入了方法");
   }
%>
```



### 7.4、JSP指令

```jsp
//配置错误页面
<%@page errorPage="error/500.jsp" %>
//会将两个页面和二为一
<%@include file=""%>
//拼接页面，本质还是三个
<jsp:include page="">
```



### 7.5、JSP内置对象及作用域

#### 7.5.1、九大内置对象

- PageContext	存东西
- Request    存东西
- Response
- Session    存东西
- Application【ServletContext】    存东西
- config   【ServletConfig】 
- out 
- page
- exception

#### 7.5.2、四大作用域

![image-20200503232002028](../../AppData/Roaming/Typora/typora-user-images/image-20200503232002028.png)



### 7.6、JSP标签、JSTL标签、EL表达式

#### EL表达式：${}

- 获取数据
- 执行运算
- 获取web开发的常用对象
- 调用Java方法



#### JSP标签

```jsp
<jsp:forward page="/jsptag2.jsp">
	<jsp:forward name="name" value="hc"></jsp:forward>
    <jsp:forward name="age" value="12"></jsp:forward>
</jsp:forward>
```



#### JSTL表达式

JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义许多标签，可以共我们使用，标签的功能和Java代码一样！



##### 核心标签

![image-20200504154901316](../../AppData/Roaming/Typora/typora-user-images/image-20200504154901316.png)

##### JSTL标签使用步骤

- 引入对应的taglib
- 使用其中的方法



## 8、JavaBean

**实体类**

JavaBean有特定的写法：

- 必须有一个无参构造
- 属性必须私有
- 必须有对应的get/set方法

一般用来和数据库的字段做映射



ORM：对象关系映射

- 表-->类
- 字段-->属性
- 行记录



## 10、Filter

Filter：过滤器，用来过滤网站的数据

- 处理中文乱码
- 登录验证



### 10.1、Filter开发步骤：

1. 导包

   ```xml
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>javax.servlet-api</artifactId>
       <version>3.1.0</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.0</version>
   </dependency>
   <dependency>
       <groupId>jstl</groupId>
       <artifactId>jstl</artifactId>
       <version>1.2</version>
   </dependency>
   <dependency>
       <groupId>taglibs</groupId>
       <artifactId>standard</artifactId>
       <version>1.1.2</version>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connection-java</artifactId>
       <version>5.1.6</version>
   </dependency>
   ```

2. 编写过滤器

   1. ```java
      public class CharacterEncodingFilter implements Filter {
          //初始化方法
          public void init(FilterConfig filterConfig) throws ServletException {
              System.out.println("CharacterEncodingFilter初始化");
          }
      
          //chain : 链
          //业务方法
      	/*
      	1.过滤器中的所有代码，在过滤器特定请求的时候都会执行
      	2.必须要让过滤器继续同行
          */
          public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
              request.setCharacterEncoding("utf-8");
              response.setCharacterEncoding("utf-8");
              response.setContentType("text/html");
      
              System.out.println("CharacterEncodingFilter执行前");
              chain.doFilter(request,response);//让我们的请求继续走，如果不写，程序到这里就被拦截停止了
              System.out.println("CharacterEncodingFilter执行后");
          }
      
          //销毁方法
          public void destroy() {
              System.out.println("CharacterEncodingFilter销毁了");
          }
      }
      ```

3. 在web.xml中配置Filter

   ```xml
   <filter>
       <filter-name>characterEncodingFilter</filter-name>
       <filter-class>com.hc.filter.CharacterEncodingFilter</filter-class>
   </filter>
   <filter-mapping>
       <filter-name>characterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```



## 11、Listener：监听器

实现一个监听器的接口；（有N中）

```java
//统计网站在线人数
public class OnlineCountListener implements HttpSessionListener {
    //创建session监听
    //一旦创建一个Session，就会触发一次这个方法
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext servletContext = se.getSession().getServletContext();
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");
        if(onlineCount == null){
            onlineCount = new Integer(1);
        }else {
            int count = onlineCount.intValue();

            onlineCount = new Integer(++count);
        }
        System.out.println(se.getSession().getId());
        servletContext.setAttribute("OnlineCount",onlineCount);
    }

    //销毁session监听
    //一旦销毁一个Session，就会触发一次这个方法
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext servletContext = se.getSession().getServletContext();
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");
        if(onlineCount == null){
            onlineCount = new Integer(0);
        }else {
            int count = onlineCount.intValue();

            onlineCount = new Integer(--count);
        }
        servletContext.setAttribute("OnlineCount",onlineCount);
    }
}
```



## 12、过滤器、监听器常见应用

用户登录之后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录后，向Session中放入用户的数据

2. 进入主页的时候，要判断用户是否已经登录；要求要在过滤器中实现

   ```java
   public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
       HttpServletRequest request1 = (HttpServletRequest)request;
       HttpServletResponse response1 = (HttpServletResponse) response;
   
       Object user_session = request1.getSession().getAttribute(Constant.USER_SESSION);
       if(user_session==null){
           response1.sendRedirect("/error.jsp");
       }
   
       chain.doFilter(request,response);
   }
   ```

