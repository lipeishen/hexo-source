title: Android's problem
date: 2014-06-09 21:31:52
tags: Android
language: ch
categories: Android
---
<!--more-->
### Android 工程中main.xml文件中提示Attribute is missing the Android namespace prefix 错误的解决办法：
 
 今天编写XML文件时，出现了Attribute is missing the Android namespace prefix的错误，开始一直找没找出原因，后来仔细一看原来只是一个很简单的单词书写错误，android写成了andriod了。
 
 
 ###怎么让eclipse 连接真实设备
 
 首先打开你的设备，连接电脑，在安卓设备上找到 设置-> 开发人员选项，选中你要使用的开发功能，安装驱动程序。
 
 然后打开eclipse, 在 window->show view->other->Android->Devices ,选中你的设备名称就行了。
 
 怎么让项目在你的设备中运行呢？只要连好设备，直接运行项目，它会再次让你选择需要运行的目标位置，你需要再次选择你的真实设备就行了。
 
 最好看看你的设备Android 版本，最好和开发的SDK中版本一直，否则会警告，但是也能运行。