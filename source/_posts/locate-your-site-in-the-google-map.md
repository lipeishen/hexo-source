title: locate your site in the google map
date: 2014-04-23 09:50:28
tags: Html5
language: ch
categories: 技术
---
<!--more-->
 主要本人运用HTML 5标签和JavaScript技术，将google map 嵌入自己的博客中，实现定位的功能，步骤如下：
 
 本人想把地图作为widget显示，所以先在\themes\freemind\layout\_widget 中创建了自己的模板文件 locate.ejs
 
 代码如下：
 ```
  <div class="widget tag">
    <h3 class="title">定位</h3>
    <p id="demo">点击这个按钮，获得您的位置：</p>
  <button onclick="getLocation()">试一下</button>
  <div id="mapholder"></div>
  <script src="http://maps.google.com/maps/api/js?  sensor=false"></script>
  <script  type="text/javascript"  src="/js/locate.js">
  </script>
  </div>
```
然后在 \themes\freemind\source\js路径中创建JS文件 locate.js 主要是通过google map api 获取当前的经度和纬度，作为google map 定位的参数。

locate.js的代码如下：
```
 var x=document.getElementById("demo");
function getLocation()
  {
  if (navigator.geolocation)
    {
    navigator.geolocation.getCurrentPosition(showPosition,showError);
    }
  else{x.innerHTML="Geolocation is not supported by this browser.";}
  }

function showPosition(position)
  {
  lat=position.coords.latitude;
  lon=position.coords.longitude;
  latlon=new google.maps.LatLng(lat, lon)
  mapholder=document.getElementById('mapholder')
  mapholder.style.height='250px';
  mapholder.style.width='250px';

  var myOptions={
  center:latlon,zoom:14,
  mapTypeId:google.maps.MapTypeId.ROADMAP,
  mapTypeControl:false,
  navigationControlOptions:{style:google.maps.NavigationControlStyle.SMALL}
  };
  var map=new google.maps.Map(document.getElementById("mapholder"),myOptions);
  var marker=new google.maps.Marker({position:latlon,map:map,title:"You are here!"});
  }

function showError(error)
  {
  switch(error.code) 
    {
    case error.PERMISSION_DENIED:
      x.innerHTML="User denied the request for Geolocation."
      break;
    case error.POSITION_UNAVAILABLE:
      x.innerHTML="Location information is unavailable."
      break;
    case error.TIMEOUT:
      x.innerHTML="The request to get user location timed out."
      break;
    case error.UNKNOWN_ERROR:
      x.innerHTML="An unknown error occurred."
      break;
    }
  }```
  
  最后在主题的配置文件 _config.yml文件中，找到widgets:
  
  在下面添加 -locate 就ok 了。