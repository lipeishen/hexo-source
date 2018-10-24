title: NODEJS
date: 2014-03-28 17:31:31
tags: hexo
categories: Node.js

---
 <!--more-->
 ##    Windows 系统下安装node.js 及其配置环境
 
   
     
###1.	下载Node.js安装文件  

 下载地址：官网http://www.nodejs.org/download/     
 
  
下载之后直接双击应用程序就会安装，安装后会自动的在环境变量中添加node.js的path.
为了测试是否已经安装成功，打开命令提示符，然后输入node, 就会进入Node.js 的交互模式，否则说明没装成功，重新再安装。

   

###2 安装git


  Node.js开发中还需要使用外部模块完成特定的功能，这需要npm来管理node.js 的模块，npm 需要安装git  http://code.google.com/p/msysgit/downloads/list  下载稳定的版本安装。
  

###3  安装Hexo 一个轻量级的博客框架


  打开git  ，输入npm install -g hexo  直到hexo 安装完成。当完成上一步之后，输入 hexo init <folder>,来创建你的项目文件夹并初始化， 里面内容是：

   + .
   + |--- _config.yml
   + |--- package.json
   + |--- scaffolds
   + |--- scripts
   + |--- source
   +   *|--- _drafts
   +   *|--- _posts
   + |--- themes


###4  打开_config.yml 文件，根据自己的情况，配置文件内容。

  系统最后要托管到github上面，所以最后您还要在github 上注册一个账号，网址是http://pages.github.com/  
   还会要求你设置自己的域名， 一般以github.io结尾。
  

_config.yml文件的最后部署部分如下，

+ deploy:
 + type:   github 
 + repository: https://github.com/lipeishen/lipeishen.github.io.git
 + branch:    master
  
  
_config.yml文件配好之后，打开git ,  进入你刚才init 的文件夹， 然后 输入如下命令： hexo deploy  ,会提示你输入用户名和密码，就是您刚才注册的github 的用户名和密码。


完成以后，输入 hexo generate ,生成本地的静态文件，最后输入hexo server 启动服务器。

最后，在浏览器中输入你自己设置的域名，就会出现博客最原始的框架了。浏览器最后使用google 的chrome .

*更改为您自己满意的全局配置后，deploy到github上。有可能出现的问题是乱码，如果写中文乱码，将存在中文的文件保存为UTF-8 无签名格式即可解决问题。*