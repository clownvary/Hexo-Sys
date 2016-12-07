title: angular2新手上路
date: 2016-12-04 00:52:49
tags: [angular2, 新手注意, 学习]
---

### 配置
- webpack　打包出错can not find name [看这个](http://stackoverflow.com/questions/33332394/angular-2-typescript-cant-find-names)
- webpack-dev-server　如果以这种方式运行，不会生成物理文件，只会在内存中生成，配合HＭR开发体验较好

- tsconfig设置

	

- tslint设置
	
	`tslint -i`生成默认配置文件
### 概念
- ngModule内的***imports***和ts文件头的***import***不同，前者是angular内部的如何组织angular特性，后者是导入文件模块然后可以外部访问
- module中的provider提供的是应用级的服务，这里生声明的在整个应用都能访问到
- 依赖注入有两种方式，一种全局注入，一种组件内注入,全局的都用到的在ｍodule内的providers声明，组件内的在组件内的providers声明
***两种注入方式都必须在头部声明import　xxx***
　有４种提供商注册方式    
	```
	　providers：［Logger］//相当于注册的key(id)和提供者一样
        providers:[{provide:Logger,useClass:UserLogger}]//key是Logger,但注入的是一个名为UserLogger的类，
        providers:[{provide:Logger,useFactory:FacLogger}]//动态产生最终的注入者
        providers:[{provide:Logger,useValue:'3'}]//注入值就是３

	```
- 组件嵌套,如果A组件在它内部使用了组件B,那么不用在A的头部import 组件B,只需要在module启动模块文件头import 组件B,同时声明declaration就行,这样全局都能访问
### 结构
-   每个项目必须有一个appModule,也就是根模块，用来告诉angular怎么组织各种模块和指令等
-   最好有一个main(boot)文件用来告诉做启动入口，之后相应的模块加载器system(webpack)会按照自己的方式导入到首页index，然后启动
