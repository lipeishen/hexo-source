title: clock
date: 2014-06-10 18:30:06
tags: clock
language: ch
categories: 技术
---
<!--more-->
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML5环形时钟</title>
    <style>
    .myCanvas {
        position: absolute;
        left: 50%;
        top: 50%;
        margin-left: -255px;
        margin-top: -255px;
        border: 5px solid #0055aa;
        font-weight:bold;
        font-size:24px;
     }
    </style>
</head>
<body>
<canvas id="myCanvas" class="myCanvas" width="500px" height="500px"></canvas>
<script>
function draw() {
    var can = document.getElementById("myCanvas");
    var canvas = can.getContext("2d");

    if (canvas) {
        canvas.fillStyle="#F00";
        canvas.font="18px Calibri";
        var time = new Date();
        var i = time.getSeconds() * 6 - 90;

        if (i == -90) {
            i = 270;
        }
        canvas.clearRect(0, 0, 500, 500);
        canvas.beginPath();
        canvas.strokeStyle = "#6AF";
        canvas.fillText(time.getSeconds()+"S",415,250);
        canvas.lineWidth = 30;
        canvas.arc(250, 250, 150, -90 * Math.PI / 180, i * Math.PI / 180, false);
        canvas.stroke();

        var j = time.getMinutes() * 6 - 90;
        if (j == -90) {
            j = 270;
        }
        canvas.beginPath();
        canvas.strokeStyle = "#5FB";
        canvas.lineWidth = 30;
        canvas.fillText(time.getMinutes()+"M",355,250);
        canvas.arc(250, 250, 90, -90 * Math.PI / 180, j * Math.PI / 180, false);
        canvas.stroke();


        var k = (time.getHours() % 12) * 30 - 90;
        if (k == -90) {
            k = 270;
        }
        canvas.beginPath();
        canvas.strokeStyle = "#8F8";
        canvas.lineWidth = 30;
        canvas.fillText(time.getHours()+"H",295,250);
        canvas.arc(250, 250, 30, -90 * Math.PI / 180, k * Math.PI / 180, false);
        canvas.stroke();


    } else {
        console.log("this is Error")
    }

}
setInterval(draw, 500);
window.onload=draw;
</script>
</body>
</html>
```
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>HTML CLOCK</title>
</head>
<body>
	<canvas width="500" height="500" id="clock">
		你的浏览器不支持canvas标签，时针显示不出来哦！
	</canvas>
	
	<script type="text/javascript">
	//画布背景颜色
	var clockBackGroundColor = "#ABCDEF";
	//时针颜色
	var hourPointColor = "#000";
	//时针粗细
	var hourPointWidth = 7;
	//时针长度
	var hourPointLength = 100;
	//分针颜色
	var minPointColor = "#000";
	//分针粗细
	var minPointWidth = 5;
	//分针长度
	var minPointLength = 150;
	//秒针颜色
	var secPointColor = "#F00";
	//秒针粗细
	var secPointWidth = 3;
	//秒针长度
	var secPointLength = 170;
	//表盘颜色
	var clockPanelColor = "#ABCDEF";
	//表盘刻度颜色
	var scaleColor = "#000";
	//表盘大刻度宽度 3 6 9 12
	var scaleBigWidth = 9;
	//表盘大刻度的长度
	var scaleBigLength = 15;
	//表盘小刻度的宽度
	var scaleSmallWidth = 5;
	//表盘小刻度的长度
	var scaleSmallLength = 10;
	//圆心颜色
	var centerColor = 'red';
	
	
	//时钟画布
	var clock = document.getElementById('clock');
	clock.style.background = clockBackGroundColor;
	//时针画布的作图环境（画板）
	var panel = clock.getContext('2d');
	
	
	//画线
	/**
	*画线段
	*
	*
	*/
	function drowLine(p,width,color,startX,startY,endX,endY,ran,cX,cY){
		//保存传入的画板，相当于每次作画新开一个图层
		p.save();
		//设置画笔宽度
		p.lineWidth = width;
		//设置画笔颜色
		p.strokeStyle = color;
		//新开启作图路径，避免和之前画板上的内容产生干扰
		p.beginPath();
		p.translate(cX,cY);
		//旋转
		p.rotate(ran);
		//移动画笔到开始位置
		p.moveTo(startX,startY);
		//移动画笔到结束位置
		p.lineTo(endX,endY);
		//画线操作
		p.stroke();
		//关闭作图路径，避免和之后在画板上绘制的内容产生干扰
		p.closePath();
		//在传入的画板对象上覆盖图层
		p.restore();
	}
	/**
	*画水平线
	*/
	function drowHorizontalLine(p,width,length,color,startX,startY,ran,cX,cY){
		drowLine(p,width,color,startX,startY,startX+length,startY,ran,cX,cY);
	} 
	/**
	*画圈圈
	*/
	function drowCircle(p,width,color,centreX,centreY,r){
		p.save();
		//设置画笔宽度
		p.lineWidth = width;
		//设置画笔颜色
		p.strokeStyle = color;
		//新开启作图路径，避免和之前画板上的内容产生干扰
		p.beginPath();
		//画圈圈
		p.arc(centreX,centreY,r,0,360,false);
		//画线操作
		p.stroke();
		//关闭作图路径，避免和之后在画板上绘制的内容产生干扰
		p.closePath();
		//在传入的画板对象上覆盖图层
		p.restore();
	}
	function drowPoint(p,width,color,centreX,centreY,r){
		p.save();
		//设置画笔宽度
		p.lineWidth = width;
		//设置画笔颜色
		p.fillStyle = color;
		//新开启作图路径，避免和之前画板上的内容产生干扰
		p.beginPath();
		//画圈圈
		p.arc(centreX,centreY,r,0,360,false);
		//画线操作
		p.fill();
		//关闭作图路径，避免和之后在画板上绘制的内容产生干扰
		p.closePath();
		//在传入的画板对象上覆盖图层
		p.restore();
	}
	function drowScales(p){
		//画小刻度
		for(var i = 0;i < 60;i++){
			drowHorizontalLine(p,scaleSmallWidth,scaleSmallLength,scaleColor,195-scaleSmallLength,0,i*6*Math.PI/180,250,250);
		}
		//画大刻度
		for(var i = 0;i < 12;i++){
			drowHorizontalLine(p,i%3==0?scaleBigWidth*1.2:scaleBigWidth,i%3==0?scaleBigLength*1.2:scaleBigLength,scaleColor,195-scaleBigLength,0,i*30*Math.PI/180,250,250);
			//可以添加数字刻度
		}
	}
	function drowHourPoint(p,hour){
		drowHorizontalLine(p,hourPointWidth,hourPointLength,hourPointColor,-10,0,(hour-3)*30*Math.PI/180,250,250);
	}
	function drowMinPoint(p,min){
		drowHorizontalLine(p,minPointWidth,minPointLength,minPointColor,-15,0,(min-15)*6*Math.PI/180,250,250);
	}
	function drowSecPoint(p,sec){
		drowHorizontalLine(p,secPointWidth,secPointLength,secPointColor,-15,0,(sec-15)*6*Math.PI/180,250,250);
	}
	function drowClock(){
		panel.clearRect(0,0,500,500);
		
		panel.fillText("联系作者 Kerbores@gmail.com",10,20);
		panel.fillText("http://kipy.xicp.net",10,40);
		var date = new Date();
		var sec = date.getSeconds();
		var min = date.getMinutes();
		var hour = date.getHours() + min/60;
		drowCircle(panel,7,'blue',250,250,200);
		drowScales(panel);
		
		drowHourPoint(panel,hour);
		drowMinPoint(panel,min);
		drowSecPoint(panel,sec);
		
		drowPoint(panel,1,centerColor,250,250,7);
		//drowHorizontalLine(panel,10,10,'red',-5,0,0,250,250);
	}
	//drowHorizontalLine(panel,7,30,'#F00',0,0,Math.PI,250,250);
	drowClock();
	setInterval(drowClock,1000);
	function save(){
		var image = clock.toDataURL("image/png").replace("image/png", "image/octet-stream"); 
		location.href=image;
	}
</script>
</body>
</html>
```

```
<!DOCTYPE HTML>
<html lang="en-US">
<head>
	<meta charset="UTF-8">
	<title></title>
	<style>
	 	  body{background: #314B62}
		  canvas{background: #eee;display: block;margin: 30px auto}
	</style>
</head>
<body>
<canvas height='400px' width='400px' id='canvas'></canvas>


<script>
   var  oCanvas = document.getElementById( 'canvas' );
   var cxt = oCanvas.getContext( '2d' );  

   function Clock(){
      this.date = new Date();
      this.h = this.date.getHours();
      this.m = this.date.getMinutes();
      this.s = this.date.getSeconds();
      this.radius = 150; //半径
      this.drawDial();
      this.drawHours();
      this.drawMinutes();
      this.drawSecond();
    //  this.drawText();
      this.updateTime();
   };
   Clock.prototype.updateTime = function() {//更新时间
      this.date = new Date();
      this.h = this.date.getHours();
      this.m = this.date.getMinutes();
      this.s = this.date.getSeconds();
      cxt.clearRect(0,0,400,400)
     
      this.drawDial();
      this.drawHours();
      this.drawMinutes();
      this.drawSecond();
     // this.drawText();
      var self = this;
      setTimeout(function(){
           self.updateTime();
      },1000);
   };
   Clock.prototype.drawText = function() {//写入文字
       cxt.fillStyle="#000";
       cxt.font="16px Microsoft YaHei";
       cxt.textBaseline="middle";
       cxt.fillText("author:涛涛",200,270);
   };
   Clock.prototype.drawDial =  function() { //画表盘
      cxt.fillStyle = '#E3E0E0';
      cxt.beginPath();
      cxt.lineWidth = 3;
      cxt.lineCap = "round"; 
      cxt.arc( 200, 200, this.radius, 0, Math.PI*2, false);
      cxt.shadowColor = "#526A89";
      cxt.shadowOffsetX = 2;   //在X轴上偏移
      cxt.shadowOffsetY = 2;   //在Y轴上偏移
      
      cxt.shadowBlur = 4;    //高斯模糊  
      cxt.closePath();
      cxt.fill();
      cxt.strokeStyle = '#313136';
      cxt.stroke();
      
      cxt.fillStyle="#3E3B3B";  //设置文字
      cxt.font="13px Arial";
      cxt.textBaseline="middle";
      cxt.textAlign='center';

      for( var i = 0; i < 60; i++ ) {
           if( i%5 == 0 ){ //画表盘时钟刻度
                 cxt.beginPath();
                  cxt.lineWidth = 2;
                 cxt.moveTo( this.getPos(12,i,this.radius)[0], this.getPos(12,i,this.radius)[1] );
                 cxt.lineTo( this.getPos(12,i,this.radius-14)[0], this.getPos(12,i,this.radius-14)[1] );
                 cxt.stroke();
                 cxt.fillText( i/5 == 0 ? 12 : i/5, this.getPos(12,i/5,this.radius-28)[0], this.getPos(12,i/5,this.radius-28)[1] );
           }else{  //画表盘秒钟刻度
                 var point1 = this.getPos( 60, i, this.radius );
                 var point2 = this.getPos( 60, i, this.radius-8 );
                 cxt.beginPath();
                 cxt.lineCap = 'square';
                 cxt.moveTo( point1[0], point1[1] );
                 cxt.lineTo( point2[0], point2[1] );
                 cxt.stroke();
           }
      };
   };
   Clock.prototype.getPos = function( n, i, l ) { //n 个数 i 索引, l 长度(取反)
         var hd =  Math.PI/180 * 360/n * i;
         var x = 200 + Math.sin( hd ) * l;
         var y = 200- Math.cos( hd ) * l; 
         return [x,y];
   };
   Clock.prototype.drawHours = function() {//画时钟
         var pos = this.getPos( 12, this.h, 35 );

         cxt.lineCap = "round"; 
         cxt.lineWidth = 2 ;
         cxt.moveTo( 200, 200 );
         cxt.lineTo( pos[0], pos[1] );
         cxt.stroke();
   };
   Clock.prototype.drawMinutes  = function() {//画分钟
         var pos = this.getPos( 60, this.m, 65);

         cxt.lineWidth = 2;
         cxt.moveTo( 200, 200 );
         cxt.lineTo( pos[0], pos[1] );
         cxt.stroke();
   };
   Clock.prototype.drawSecond = function() {//画秒钟
         var pos = this.getPos( 60, this.s, 95);
         cxt.moveTo( 200, 200 );
         cxt.lineTo( pos[0], pos[1] );
         cxt.stroke();
   };


   var taotao = new Clock();

</script>
</body>
</html>
```