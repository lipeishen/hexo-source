title: Java Standard Tag Library 
date: 2014-07-11 21:40:24
tags: JSTL
language: ch
categories: Java
---
<!--more-->
###核心标签库
```
<c:out>  
<c:set> 
<c:remove> 
<c:catch>
<c:if>    
<c:choose>(多条件判断 可以设置<c:when> 和<c:otherwise>)
<c:forEach> 输出数组、集合
<c:forTokens> 字符串拆分及输出操作
<c:import>  将指定的路径包含到当前页面显示
<c:url>     根据路径和参数生成一个新的URL
<c:redirect>  客户端跳转
```

###格式化标签库

####国际化标签
```
<fmt:setLocale>      设置一个全局的地区代码
<fmt:requestEncoding>  设置统一的请求编码
 ``` 
####信息显示标签
```
<fmt:bundle> 设置临时的要读取资源文件名称
<fmt:message>  通过key 取得value, 通过<fmt:param>向动态文本中设置内容
<fmt:setBundle>  设置一个全局的要读取资源文件名称
```
####数字及日期格式化
```
<fmt:formatNumber>  格式化数字 （变成字符串）
<fmt:parseNumber>   反格式化数字
<fmt:formatDate>
<fmt:parseDate>
<fmt:setTimeZone> 设置一个全局时区
<fmt:timeZone>     设置一个临时时区
```
###SQL标签库

```<sql:setDataSource>
<sql:query>
<sql:update>
<sql:transaction>
```
###XML 标签库
```
<x:out>  输出XPath 指定的内容
<x:parse>  进行XML解析
<x:set>   将内容保存在属性范围中
<x:if>
<x:choose>
<x:when>
<x:otherwise>
<x:forEach>
```

###函数标签库
```
${fn:contains()}
${fn:containsIgnorceCase()}
${fn:startsWith()}
${fn:endsWith()}
${fn:toUpperCase()}
${fn:toLowerCase()}
${fn:substring()}
${fn:split()}
${fn:join()}
${fn:escapeXml()}  将 <  >  " '等替换成转义字符
${fn:trim()}
${fn:replace()}
${fn:indexOf()}
${fn:substringBefore()}
${fn:substringAfter()}
```

关于泛型的问题，主要是解决集合中的对象，取出时变为Object， 还需强制转化为原类型，解决办法就是使用泛型<>

```list<String> list= ArrayList<String>();```

Java 中集合主要分为两类接口 ` Collection` 和`Map, Collection `又分为`Set、Queue、和List`三类接口，其中`Set` 是无序的不能有重复的元素，`List` 可以。

泛型中`List<Collection>` 并不是`List<set>`的父类，这一点一定要搞明白。一般使用通配符`“？”List<?>,`它可以是任意`List<>`的父类，要想限制子类，需要
`List<? extends Collection>`