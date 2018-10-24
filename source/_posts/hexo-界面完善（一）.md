title: hexo 界面完善（一）
date: 2014-04-04 20:41:57
tags: hexo
language: ch
categories: hexo
---
<!--more-->
##今天主要完成了的任务：


###1.	 Fork me on GiHub图标的添加 
这里 https://github.com/blog/273-github-ribbons 有 github 给出的教程，把代码插入到任意一个全局的模板文件中就行，比如layout.ejs的末尾。注意把网址https://github.com/you  **you**要换成你在github 上的名字
###2.	去掉了右上角导航栏中默认的搜索框 
代码在 \pacman\layout\_partial\header.ejs中，删除相关代码即可。
###3  添加娱乐功能“High 一下” 
在\pacman\layout\_partial\header.ejs中，找到一组<ul> </ul> 标签，将如下的代码添加进去就行了。

    <li> <a title="把这个链接拖到你的Chrome收藏夹工 具栏中" href='javascript:(function() {
	function c() {
		var e = document.createElement("link");
		e.setAttribute("type", "text/css");
		e.setAttribute("rel", "stylesheet");
		e.setAttribute("href", f);
		e.setAttribute("class", l);                                                       
		document.body.appendChild(e)
	}
 
	function h() {
		var e = document.getElementsByClassName(l);
		for (var t = 0; t < e.length; t++) {
			document.body.removeChild(e[t])
		}
	}
 
	function p() {
		var e = document.createElement("div");
		e.setAttribute("class", a);
		document.body.appendChild(e);
		setTimeout(function() {
			document.body.removeChild(e)
		}, 100)
	}
 
	function d(e) {
		return {
			height : e.offsetHeight,
			width : e.offsetWidth
		}
	}
 
	function v(i) {
		var s = d(i);
		return s.height > e && s.height < n && s.width > t && s.width < r
	}
 
	function m(e) {
		var t = e;
		var n = 0;
		while (!!t) {
			n += t.offsetTop;
			t = t.offsetParent
		}
		return n
	}
 
	function g() {
		var e = document.documentElement;
		if (!!window.innerWidth) {
			return window.innerHeight
		} else if (e && !isNaN(e.clientHeight)) {
			return e.clientHeight
		}
		return 0
	}
 
	function y() {
		if (window.pageYOffset) {
			return window.pageYOffset
		}
		return Math.max(document.documentElement.scrollTop, document.body.scrollTop)
	}
 
	function E(e) {
		var t = m(e);
		return t >= w && t <= b + w
	}
 
	function S() {
		var e = document.createElement("audio");
		e.setAttribute("class", l);
		e.src = i;
		e.loop = false;
		e.addEventListener("canplay", function() {
			setTimeout(function() {
				x(k)
			}, 500);
			setTimeout(function() {
				N();
				p();
				for (var e = 0; e < O.length; e++) {
					T(O[e])
				}
			}, 15500)
		}, true);
		e.addEventListener("ended", function() {
			N();
			h()
		}, true);
		e.innerHTML = " <p>If you are reading this, it is because your browser does not support the audio element. We recommend that you get a new browser.</p> <p>";
		document.body.appendChild(e);
		e.play()
	}
 
	function x(e) {
		e.className += " " + s + " " + o
	}
 
	function T(e) {
		e.className += " " + s + " " + u[Math.floor(Math.random() * u.length)]
	}
 
	function N() {
		var e = document.getElementsByClassName(s);
		var t = new RegExp("\\b" + s + "\\b");
		for (var n = 0; n < e.length; ) {
			e[n].className = e[n].className.replace(t, "")
		}
	}
 
	var e = 30;
	var t = 30;
	var n = 350;
	var r = 350;
	var i = "//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake.mp3";
	var s = "mw-harlem_shake_me";
	var o = "im_first";
	var u = ["im_drunk", "im_baked", "im_trippin", "im_blown"];
	var a = "mw-strobe_light";
	var f = "//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake-style.css";
	var l = "mw_added_css";
	var b = g();
	var w = y();
	var C = document.getElementsByTagName("*");
	var k = null;
	for (var L = 0; L < C.length; L++) {
		var A = C[L];
		if (v(A)) {
			if (E(A)) {
				k = A;
				break
			}
		}
	}
	if (A === null) {
		console.warn("Could not find a node of the right size. Please try a different page.");
		return
	}
	c();
	S();
	var O = [];
	for (var L = 0; L < C.length; L++) {
		var A = C[L];
		if (v(A)) {
			O.push(A)
		}
	}
    })()  '>动起来</a> 
 
    </li>

###4 在主题的右侧增加了友情链接功
这个比较简单，pacman 主题已经把模板写好了，\pacman\layout\_widget\links.ejs  ,在links.ejs中添加你要链接的内容就行了，格式如下：
<li><a href="http://hexo.io/index.html" target="_blank" title="Hexo">Hexo</a></li>

然后打开\pacman\_config.yml 文件，在widgets 下增加`  - links`
