# JAVA Servlet

## 9大内置对象

### Request
接受http协议传送到服务器的数据
e.g.
```
HttpServletRequest request;
request.setCharacterEncoding("utf-8");
String cu_name = request.getParameter("cu_name");
String cu_password = request.getParameter("cu_password");s
```

### Response
对客户端的响应
e.g.
```
response.setCharacterEncoding("utf-8");
response.sendRedirect("/Webapp");
```

### Session
session 对象是由服务器自动创建的与用户请求相关的对象。服务器为每个用户都生成一个session对象，用于保存该用户的信息，跟踪用户的操作状态。session对象内部使用Map类来保存数据，因此保存数据的格式为 “Key/value”。 session对象的value可以使复杂的对象类型，而不仅仅局限于字符串类型。

### Application
application 对象可将信息保存在服务器中，直到服务器关闭，application对象中保存的信息会在整个应用中都有效。与session对象相比，application对象生命周期更长，类似于系统的“全局变量”。
application对象的基类是：javax.servlet.ServletContext

### Out
out 对象用于在Web浏览器内输出信息，并且管理应用服务器上的输出缓冲区。在使用 out 对象输出数据时，可以对数据缓冲区进行操作，及时清除缓冲区中的残余数据，为其他的输出让出缓冲空间。待数据输出完毕后，要及时关闭输出流。
e.g.
```
out.write("<html>\r\n");
      out.write("<head>\r\n");
      out.write("<meta http-equiv=\"Content-Type\" content=\"text/html; charset=UTF-8\">\r\n");
      out.write("<title>Insert title here</title>\r\n");
      out.write("</head>\r\n");
```

### Page Context
pageContext 对象的作用是取得任何范围的参数，通过它可以获取 JSP页面的out、request、reponse、session、application 等对象。pageContext对象的创建和初始化都是由容器来完成的，在JSP页面中可以直接使用 pageContext对象。

（javax.servlet.jsp.PageContext的实例，对象直译时可以称作“页面上下文”对象，代表的是当前页面运行的一些属性，通过此对象可以拿到其他8大对象，使用该对象可以访问页面中的共享数据，常用的方法：getServletContext()和getServletConfigO.）

### Config
```
ServletConfig config = this.getServletConfig();
String name = config.getInitParameter("example");
```

### page
page 对象代表JSP本身，只有在JSP页面内才是合法的。 page隐含对象本质上包含当前 Servlet接口引用的变量，类似于Java编程中的 this 指针。

### exception

### Cookie
e.g.
```
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //创建Cookie
    Cookie cookie1 = new Cookie("test","haha");
    Cookie cookie2 = new Cookie("test2", "lala");
    /*
        *设置Cookie的有效时长，单位(秒)，
        * 当参数大于0,将Cookie缓存到本地硬盘上
        * 当参数等于0，Cookie一旦设置立马失效
        * 当Cookie小于0，将Cookie缓存到浏览器中
    */
    cookie1.setMaxAge(60*60*24);
    cookie2.setMaxAge(60*60*24);
    //绑定路径
    cookie1.setPath(req.getContextPath()+"/cookie1");
    cookie2.setPath(req.getContextPath()+"/cookie2");
    //向响应中添加Cookie
    resp.addCookie(cookie1);
    resp.addCookie(cookie2);

}

```