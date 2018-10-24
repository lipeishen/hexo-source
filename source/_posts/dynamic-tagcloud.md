title: Dynamic tagcloud
date: 2014-06-22 10:04:10
tags: tagcloud
language: ch
categories: 技术
---
<!--more-->
### 下载所需的东西

要创建动态的标签云，首先从 http://pan.baidu.com/s/1kT4yyDD 下载所需要的js 文件和flash ，已经有现成的东西了，所以省去了很多的精力，但是可能不利于自己成长。
大牛们基本上都是自己写出想要的东西，毕竟不是大牛，所以只好这么办了。

### 配置文件

把你下载的js 和flash 放在你的js文件夹下，这里我们要做的是一个widget,所以进入自己的layout/_widget 下创建 自己的模板文件 tagloud.ejs ,代码如下：

```
  <style type="text/css">
#cloud{background-color:#551A8B;color:#FFF}
a{text-decoration:none;}
strong{color:#FF0000}
  </style>
  
  

 <script type="text/javascript" src="js/swfobject.js"></script>
 <h4 class="tag_box"><%= __('tagcloud') %></h4> 
 <div id="cloud">
<% if (site.tags.length){ %>
   

   <div class="tagcloudlist clearfix">
     
      <script type="text/javascript">
var cloud = new SWFObject("js/tagcloud.swf", "tagcloudflash", "200", "170", "9", "#ffffff");
cloud.addParam("wmode", "transparent");
cloud.addParam("allowScriptAccess", "always");
cloud.addVariable("tcolor", "0xFFFFFF");
cloud.addVariable("tcolor2", "0xFFFFFF");
cloud.addVariable("hicolor", "0x999999");
cloud.addVariable("tspeed", "150");
cloud.addVariable("distr", "true");
cloud.addVariable("mode", "tags");

var str = '<tags>  <%- tagcloud() %> </tags>';
var newStr=encodeURIComponent(str);
cloud.addVariable("tagcloud",newStr);
cloud.write("cloud");
</script>
      
     </div>   
  
<% } %>

</div>
```

这些代码只是针对我的文件，你想要制作自己的东西，可以按照下载文件里面的index.html 文件中的说明来操作，可能与本人的不太一样，但是最后效果都一样，条条大路通罗马，哈哈，小伙伴们，
还等什么呢，赶紧行动吧。




