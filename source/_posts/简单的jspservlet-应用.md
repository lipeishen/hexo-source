title: "简单的Jsp+Servlet 应用"
id: 6
date: 2014-04-08 23:15:55
tags:  jsp
categories: Java
---
<!--more-->
开发应用的前提是配置好MyEclipse+tomcat+jdk, 具体的配置过程我就不详细说了，下面我就介绍一下创建一个简单的动态web应用的过程。


下面是结果：

&nbsp;

![1](/img/11.jpg) ![2](/img/21.jpg)

首先新建一个web工程，在src下面新建一个sunyang的包，这个包里有两个类，User.java  和 HelloUserServlet.java

代码分别是

// User.java

package sunyang;

public class User {

/**
* @param args
*/
private String username;//用户名

public String getUsername() {
return username;
}

public void setUsername(String username) {
this.username = username;
}

}

//HelloUserServlet.java

package sunyang;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloUserServlet extends HttpServlet {

protected void doGet(HttpServletRequest req, HttpServletResponse resp)
throws ServletException, IOException {
// TODO Auto-generated method stub
doPost(req, resp);
}

protected void doPost(HttpServletRequest req, HttpServletResponse resp)
throws ServletException, IOException {
// TODO Auto-generated method stub
String username=req.getParameter("username");
User user=new User();
user.setUsername(username);
//将User 对象添加到HttpServletRequest对象中
req.setAttribute("user", user);
//将请求转向制定的页面
RequestDispatcher rdt=req.getRequestDispatcher("/helloUser.jsp");
rdt.forward(req, resp);
}

}

然后再WebRoot的目录下新建名为helloUser.jsp的文件

代码如下：

//helloUser.jsp

&amp;gt;&lt;%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%&gt;

&lt;!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"&gt;
&lt;jsp:useBean id="user" scope="request" class="sunyang.User" /&gt;
&lt;html&gt;
&lt;head&gt;

&lt;title&gt;Hello&lt;/title&gt;

&lt;/head&gt;

&lt;body&gt;
&lt;%
if(user.getUsername()!=null){
if(user.getUsername()==""){

out.println("Please enter a username");

}
else{ %&gt;

hello:&lt;%=user.getUsername() %&gt;
&lt;%} } %&gt;
&lt;form action="helloUser" method="post"&gt;
&lt;input type="text" name="username" value="" /&gt;
&lt;input type="submit" name="submit" value="submit" /&gt;

&lt;/form&gt;

&lt;/body&gt;
&lt;/html&gt;

完了之后，还要把它部署到Servlet容器中去，只需在WEB_INF目录下的web.xml 文件中配置即可。增加如下内容即可实现部署：

&lt;servlet&gt;
&lt;servlet-name&gt;HelloUserServlet&lt;/servlet-name&gt;
&lt;servlet-class&gt;sunyang.HelloUserServlet&lt;/servlet-class&gt;

&lt;/servlet&gt;
&lt;servlet-mapping&gt;

&lt;servlet-name&gt;HelloUserServlet&lt;/servlet-name&gt;
&lt;url-pattern&gt;/helloUser&lt;/url-pattern&gt;

&lt;/servlet-mapping&gt;

最后把项目部署到tomcat服务器上，然后再浏览器中运行即可。

myeclipse 怎么设置让编辑器自动提示代码?

<pre id="best-content-1177886717">打开 Eclipse  -&gt; Window -&gt; Perferences -&gt; Java -&gt; Editor -&gt; Content Assist，在右边最下面一栏找到 auto-Activation ，下面有三个选项，找到第二个“Auto activation triggers for Java：”选项 

在其后的文本框中会看到一个“.”存在。这表示：只有输入“.”之后才会有代码提示和自动补全，我们要修改的地方就是这里。把该文本框中的“.”换掉，换成“abcdefghijklmnopqrstuvwxyz.”，这样，你在Eclipse里面写Java代码就可以做到按“abcdefghijklmnopqrstuvwxyz.”中的任意一个字符都会有代码提示</pre>