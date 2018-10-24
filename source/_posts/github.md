title: github
date: 2014-04-01 12:05:27
tags: GitHub
categories: GitHub
---

<!--more-->
#怎样把本地文件上传到github上


##过程可以分为两步走：

###第一步：在git下安装formidable和fs-extra，命令如下：npm install formidable , npm install fs-extra ;

然后写程序提交本地代码到后台（有了fs-extra之后，可以把后台的文件拷贝到指定的位置）

代码如下//

    var formidable = require('formidable'), 
 
    http = require('http'), 
    
    util = require('util'),
    
    
    fs   = require('fs-extra');
   
   
 
    http.createServer(function(req, res) {

  /* Process the form uploads */
  
    if (req.url == '/upload' && req.method.toLowerCase() == 'post') {
  
    var form = new formidable.IncomingForm();
    
    form.parse(req, function(err, fields, files) {
    
      res.writeHead(200, {'content-type':
      'text/plain'});
      
      res.write('received upload:\n\n');
      
      res.end(util.inspect({fields: fields, files:
      files}));
      
    });
 
//显示上传的进度
 
    form.on('progress', function(bytesReceived, bytesExpected) {
        var percent_complete = (bytesReceived / bytesExpected) * 100;
        console.log(percent_complete.toFixed(2));
    });
 
    form.on('error', function(err) {
        console.error(err);
    });
 
    form.on('end', function(fields, files) {
        /* Temporary location of our uploaded file */
        var temp_path = this.openedFiles[0].path;
        /* The file name of the uploaded file */
        var file_name = this.openedFiles[0].name;
        /* Location where we want to copy the uploaded file */
        var new_location = 'D:/tmp/test/';
 
        fs.copy(temp_path, new_location + file_name, function(err) {  
            if (err) {
                console.error(err);
            } else {
                console.log("success!")
            }
        });
    });
 
    return;
    }
 
  /* Display the file upload form. */
  
   
     res.writeHead(200, {'content-type': 'text/html'} ;
      res.end(
  
    '<form action="/upload" enctype="multipart/form-data" method="post">'+
    
    '<input type="text" name="title"><br>'+
    
    '<input type="file" name="upload"
    multiple="multiple"><br>'+
    
    '<input type="submit" value="Upload">'+
    
    '</form>'
    
    );
  
 
     }).listen(8080);

这种方法一次只能上传一个文件，文件夹还不行，效率不高。

###第二步：将本地文件同步到github上

先进入https://github.com  注册账号，

然后在github上面创建自己的仓库，可以参照下面两篇文章

http://blog.sina.com.cn/s/blog_63eb3eec0101cf6x.html

http://www.cnblogs.com/findingsea/archive/2012/08/27/2654549.html

如果在执行  　ssh -T git@github.com  时出现错误 （connect to host github.com  port num 22 Bad file number） 时，

在.ssh目录下添加一个叫做config文件，没有后缀名。

2.	Host github.com  
3.	User git  
4.	Hostname ssh.github.com  
5.	PreferredAuthentications publickey  
6.	IdentityFile ~/.ssh/id_rsa  
7.	Port 443  
问题就可以解决了

最后，把本地的文件通过 git push origin master 推送到github 上就行了。



