title: hexo文件夹配置
date: 2016-12-05 15:46:22
tags: [配置, hexo, 笔记]
---
Hexo 在写文章时，如果需要引用资源文件比如图片，可以直接使用url也可以使用本地资源，url不用说了，本地资源配置如下：
<!--more-->
有两种方法可以访问到本地资源


1.应用级资源文件夹形式

- 在source目录下建立Images文件夹，放入图片
- 文章中引用`![引用图片](/Images/xxx.png)`
- hexo s发布测试，就可以看到，如果直接预览markdown格式不行，必须要本地服务启动测试才行
- 我来自／source/Images目录![img](/Images/down.png)
	
2.子目录文件夹形式

- 在＿config.yml中修改配置项`post_asset_folder: true` 开启功能
- 之后每次hexo new ...文章时会同时生成一个同名的文件夹，把该文章的图片放进去
- 访问格式{% asset_img xxx.jpg This is an example image %},不能直接用markdown格式引入
- 我来自内部目录{% asset_img down.png this is img from folder%}
