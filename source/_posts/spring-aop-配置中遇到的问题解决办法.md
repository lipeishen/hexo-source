title: "Spring AOP 配置中遇到的问题解决办法"
id: 15
date: 2014-04-17 13:12:37
tags: Spring
categories: 
- Java
---
<!--more-->
问题：

## aop advisor说明以及The prefix "aop" for element "aop:config" is not bound.

解决：首先应该加载JAR包。

还要在&lt;beans&gt;中要加入“xmlns：aop”的命名申明，

并在“xsi：schemaLocation”中指定aop配置的schema的地址
配置文件如下：

&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans "
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance "
<span style="color: #ff9900;">xmlns:aop="http://www.springframework.org/schema/aop "</span>
xmlns:tx="http://www.springframework.org/schema/tx "
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
<span style="color: #ff9900;">  http://www.springframework.org/schema/aop</span>
<span style="color: #ff9900;">          http://www.springframework.org/schema/aop/spring-aop.xsd</span> "&gt;

一般情况下，将橙色部分加载到头部，问题就可以解决，但是有时候加上去之后，还是提示同样的问题，我们该怎么办呢？

本人经过多次尝试，终于发现了问题，解决办法如下：

首先要检查Spring的版本信息，参照文章  [http://blog.csdn.net/xiaxiaorui2003/article/details/4582778](http://blog.csdn.net/xiaxiaorui2003/article/details/4582778)

然后对照    http://www.springframework.org/schema/beans/spring-beans-2.0.xsd

http://www.springframework.org/schema/tx/spring-tx-2.0.xsd

http://www.springframework.org/schema/aop/spring-aop-2.0.xsd

这里最后的数字就是对应的版本号，改成你的Spring版本号就行了。问题解决！