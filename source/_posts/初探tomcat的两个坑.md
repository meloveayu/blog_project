---
title: 初探tomcat的两个坑
date: 2020-08-06 22:07:17
categories: 学习笔记
tags: 
	- tomcat
	- 防火墙
---

自从上次风风火火装完了java的一套后端系统，我也没仔细研究过，然后今日想试一试，然后就踩坑了……

<!--more-->

首先第一个坑，上次配置过的tomcat启动和关闭命令在运行时会报找不到JAVA_HOME和JRE_HOME的错误，然后我遍寻网络，各种说法都有，最后在tomcat的bin目录下的catalina.sh中添加了一个`export JAVA_HOME="JDK的路径"`才解决了这个问题。

第二个坑，这是上次留下的，上次配好之后我没有细究，就是无法访问IP地址下的8080端口，然后一开始以为是8080被其他程序占用了，查询情况无果。最后通过解决防火墙的端口打开才把问题解决……（也是一个教训，之前总是想着宿主机的防火墙，没想到还有一个虚拟机的防火墙）

代码如下（系统是CentOS 8）:

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload
```

第一句就是在public作用域上添加8080端口运行TCP协议，永久生效；第二句是重启防火墙。

深感自己所学的浅薄，目前粗浅地只能理解tomcat就是个装前端的容器。🤔