---
title: 用Typora外加PicGo搭配完成更舒心的博客环境
date: 2020-05-31 12:10:43
categories: 实用小文章
tags: 
- 图床
- 博客
---



在之前的博客文章书写时，自己并没有部署图床，所以在之前的文章里都没有图片插入，这次主要是部署Github图床，搭配上Typora和PicGo，可以实现直接在书写中粘贴图片同时上传图床，无脑又好用。

<!--more-->

## Github图床的部署

这里所谓的图床，就是在Github里建一个仓库专门上传照片，原理很简单。当然首先应该要有一个自己的Github账户，创建一个新的仓库，名字任意。注意要把仓库设置成公开（Public）。

创建完成后点击自己的账户头像，选择设置（Settings），再转到Developer settings，点击Personal access tokens中的Generate new token按钮，description可以自行填写，勾选repo项点击最下方的Generate token即可。

![](https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200531123047.png)

会出现一串字符，只出现一次，记得保存好。

## PicGo的安装和连接Github图床

PicGo(2.2.2版本)是一个开源图床工具，只要把图片拖入上传区，就可以实现向连接的图床上传图片的功能。

PicGo可以在Github上进行项目搜索并下载。

安装过程很简单，不再赘述。主要部分是图床的连接设置。在PicGo的图床设置界面中找到Github图床，进行设置：

![image-20200601145312914](https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200601145313.png)

> 仓库名是Github账户名字/仓库名字
>
> 分支名默认master即可
>
> Token填写之前生成的token
>
> 存储路径可以按默认提示来即可
>
> 设置自定义域名：因为Github访问的速度偏慢，可以考虑用CDN加速，使用`https://cdn.jsdelivr.net/gh/[账户名]/[仓库名]@master`来进行加速，jsdelivr可以提供对Github内容的分发，具体原理可以自行了解

经过这些配置，目前PicGo已经可以完成将图片通过上传区上传到Github图床上了。但是使用还是不便，大多数情况，要先上传图片，再复制生成的对应链接，粘贴到.md文件里。要想更省力，还得装一个Typora。

## 安装Typora和PicGo进行联动

Typora确实是我目前用过的最好的Markdown编辑器了，之前一直都在用VS Code的我流下了泪水……（当然VS Code也不错）Typora支持实时预览，预览界面和编辑界面是一体的，感觉很奇妙，毕竟写Markdown不是敲代码，稍微舒心一点怎么都好，同时最近版本（0.9.88beta win版）是支持和PicGo联动完成插入图片即完成上传的功能的，体验突然飞起。

Typora的安装包去官网下载，唯一的难点大概是，下载非常慢……安装即可。然后去菜单里的偏好设置里选择图像进行配置：

![image-20200601151704251](https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200601151704.png)

> 下拉项选择上传图片，勾选对本地和网络位置图片应用规则
>
> 上传服务设定，上传服务选择PicGo(app)，配置PicGo路径
>
> 验证图片上传选项稍后进行

![image-20200601152254152](https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200601152254.png)

打开PicGo，在PicGo配置里选择设置server，将监听端口设置为上面所示的36677（因为Typora只用这个端口传输🙁希望之后能出个自行设置端口的选项）。

此时运行Typora中的验证图片上传选项，显示为:![image-20200601152735860](https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200601152735.png)说明连接成功了，之后的图片可以直接插入，直接就会上传到图床。

## 问题

1. 显示"Failed to fetch"：上传不了图片，可以检查以下端口设置，有时打开了多个PicGo，端口冲突，就会自动把原先的端口改了，可以改回来，再关掉所有PicGo，重新上传。
2. 显示{"success",false}：上传了重名的图片，可以在PicGo设置里将时间戳重命名打开即可。
3. 更多问题可以看PicGo的日志文件找办法。