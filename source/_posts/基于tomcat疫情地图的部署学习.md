---
title: 基于tomcat疫情地图的部署学习
date: 2020-08-07 15:40:47
categories: 项目实践
tags:
	- tomcat
---

## 前言

老实说这根本算不上是个项目吧。就是自己看见小甲鱼的某个开课吧恰饭广告突然心血来潮想着就算参加了也不会掉块肉，然后就加了进去看看。

<!--more-->

总的来说东西非常简单，因为是面向0基础同学的，所以估计大部分人也就是看看，自己也算是科班出身那就更是没啥难度了。就是不求甚解地在阿里云服务器上部署一个tomcat服务，然后把前端jsp扔到容器里边，写上不到10行代码，再申请一个微信订阅号，整一个自动回复，就完成了。我上午磨磨叽叽边复习政治边做，走上正轨后也就是花了个大概一个多小时就完事了。

## 具体步骤

1. 整一台服务器，我用的是2月份从阿里云白嫖的服务器，还有二十来天可用，然后自己一开始想用自己的虚拟机来做，不过期间问题不少。就放弃了。

2. 装JDK和tomcat，这个部分我之前参考着程序羊的手册在虚拟机上做了一遍，这个部分不难。照做就行，昨天踩的坑都总结了。

3. 配置访问端口，tomcat默认页面是8080端口访问，改成80主要是为了用http访问。修改tomcat目录conf文件夹里面server.xml的connector项，打开一眼就能看到"8080"，改了就行。

4. 云服务器相比虚拟机好的一点就是配置出入规则太方便了，想打开哪个端口也不用敲命令。打开80端口的访问权限后http访问，tomcat打开的情况下直接就会显示默认页面。

5. 把前端提供的脚本和页面添加到webapps文件夹里面去，脚本直接丢进去，页面就是复制然后替换index.jsp文件，保留编码规定的那一行。现在访问无法获取数据，要想动态获取数据就是给index.jsp添加java控制代码：

   ```html
   <script type="text/javascript">
         <%
           //获取疫情数据的网址
           URL url = new URL("https://zaixianke.com/yq/all");
             URLConnection conn = url.openConnection();
             //获取输入流
             InputStream in = conn.getInputStream();
             //包装输入流为字符流进一步包装为缓冲字符流
             BufferedReader reader = new BufferedReader(new InputStreamReader(in, StandardCharsets.UTF_8));
             //读取一行内容
             String str = reader.readLine();
       	  //这句没用
             System.out.println(str);
         %>
           var data = <%=str%>;
       </script>
   ```

   这个部分把Java代码嵌入到页面完成了个数据读取的功能，说实话现在前端还用JSP这东西吗？Java代码当然包还是要导入的，导入的部分也在index.jsp里实现：

   ```html
   //就是上面要求保留的编码规定的那一行
   <%@ page session="false"
   import="java.io.BufferedReader,java.io.IOException,java.io.InputStream,java.io.InputStreamReader,java.net.URL,java.net.URLConnection,java.nio.charset.StandardCharsets"
   pageEncoding="UTF-8" contentType="text/html; charset=UTF-8" %>
   ```

   现在再访问，结果就是动态的了。

6. 申请个微信订阅号，做个自动留言。这个太简单了，只不过是个表现形式而已，申请一遍就无师自通了。

<img src="https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200807163428.png"  />
<img src="https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200807163450.png"  />



## 总结

基本没讲什么的水课，唯一的用处大概就是让我用了用云服务器的服务部署，了解了一下tomcat和JSP，其他的感觉就和你在bilibili上找个网课看效果差不多，编程这个行当确实就是得多练多学多敲多输出，不是你整一个网课学过了就以为学到了。

个人竟然被这个动摇了考研的心志……惭愧。考研还是为了见识更广，二十多岁正是学习的好时机，为什么要忙着去赚钱呢？

材料扔到网盘里了，这只是个部署，也没必要扔到github上。

进步一点点。