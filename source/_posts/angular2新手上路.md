title: angular2新手上路
date: 2016-12-04 00:52:49
tags: [angular2, 新手注意, 学习]
---

### 配置
- webpack　打包出错can not find name [看这个](http://stackoverflow.com/questions/33332394/angular-2-typescript-cant-find-names)
- webpack-dev-server　如果以这种方式运行，不会生成物理文件，只会在内存中生成，配合HＭR开发体验较好
- webstorm中不要开启typescript的`use typescript service(experimental)`功能,会语法提示错误
- webstorm 中导入component时，`alt+enter`可以自动导入当前文件中用到的组件
- webpack2和1的配置有很大不同,注意看版本号,不能使用简写了所有的loader必须添加"xxx-loader"
- webpack2中 ExtractTextPlugin必须使用loader,不能用use,否则出错`loader: ExtractTextPlugin.extract({fallbackLoader: "style-loader", loader: "css-loader"})`
- tsconfig设置


- tslint设置
	
	`tslint -i`生成默认配置文件
	
### 概念
- ngModule内的***imports***和ts文件头的***import***不同，前者是angular内部的如何组织angular特性(即需要angular内的什么特性就声明什么,比如每个都需要的BrowserModule,RouteModule等,只能是ngModule)，后者是导入文件模块然后可以外部访问
- module中的provider提供的是应用级的服务，这里声明的在整个应用都能访问到
- 依赖注入有两种方式，一种全局注入，一种组件内注入,全局的都用到的在ｍodule内的providers声明，组件内的在组件内的providers声明
***两种注入方式都必须在头部声明import　xxx***
　有４种提供商注册方式    
	```
	　providers：［Logger］//相当于注册的key(id)和提供者一样
        providers:[{provide:Logger,useClass:UserLogger}]//key是Logger,但注入的是一个名为UserLogger的类，
        providers:[{provide:Logger,useFactory:FacLogger}]//动态产生最终的注入者
        providers:[{provide:Logger,useValue:'3'}]//注入值就是３

	```
- HTML attribute value指定了初始值；DOM value property 是当前值,两个不同,angular中主要是用到property,不是attribute,
- 安全导航操作符 ( ?. ) 和空属性路径,用来避免null或者undefine时程序崩溃,当不确定对象是否有此属性时使用

   	`The null hero's name is {{nullHero?.firstName}}`
   相当于
   	`<span *ngIf='nullHero'>The null hero's name is</span>`
- 组件嵌套,如果A组件在它内部使用了组件B,那么不用在A的头部import 组件B,只需要在module启动模块文件头import 组件B,同时声明declaration就行,这样全局都能访问
- 用户输入,模板引用变量
	```
	//以#开头直接在模板中使用变量
	@Component({
  	selector: 'loop-back',
  	template: `
    	<input #box (keyup)="0">
    	<p>{{box.value}}</p>
  	`
	})
	```
- 数据绑定可单向可双向,单向由模型=>视图`属性绑定<div [name]='model.name'>`,`插值绑定<div name='{{model.name}}''>`,两者一样,,由视图=>模型(其实就是事件响应)`<div (click)='model.clickHandle'>`使用括号,双向绑定的话使用`[(xxx)]`
- `属性绑定<div [name]='model.name'>`,`插值绑定<div name='{{model.name}}''>`,两者都绑定的是属性(property)不是attribute,要设置attribute,如下
`<td [attr.colspan]="1 + 1">`,使用`attr.`语法
- 双向绑定ngModel,`<input [(ngModel)='xxx']`,自定义的指令没有ngModel属性,除非自己设置了值访问器.
相当于

	```
	//[(ngModel)]语法只能设置一个数据绑定属性。 如果需要做更多或不同的事情，就得自己用它的展开形式。
	//强制让输入大写
	<input
  	[ngModel]="currentHero.firstName"
  	(ngModelChange)="setUpperCaseFirstName($event)">
	```
- 内置指令
  
  `*ngIf`,`*ngFor`,`[ngSwitch],*ngSwitchCase`,注意`[ngSwitch]`是[ ]没有`*ngFor`,的`trackBy`是一个函数
- `<div [class.special]="isSpecial">`,设置单个class,设置多个class时使用`<div [ngClass]="{isSpecial:false,isSpecial2:false}">`
- 自定义组件的属性必须显示声明输入或输出,输入就是自定义的指令属性,输出就是事件响应,事件参数默认是`$event`

	```
	<hero-detail [hero]="currentHero" (deleteRequest)="deleteHero($event)">
	//方法一定义,注意是在导出的类中
	export class xxxComponent{
	@Input()  hero: Hero;
	@Output() deleteRequest = new EventEmitter<Hero>();
	}
	//指定别名,	外部使用myclick,内部使用clicks
	@Output('myClick') clicks = new EventEmitter<string>(); //  @Output(alias) propertyName = ...
	//方法二定义,注意是在组件声明中
	@Component({
  	inputs: ['hero'],
  	outputs: ['deleteRequest'],
	})
	//指定别名
	@Directive({
  	outputs: ['clicks:myClick']  // propertyName:alias
	})

	```
- ***两种注入方式都必须在头部声明import　xxx***

### 结构
-   每个项目必须有一个appModule,也就是根模块，用来告诉angular怎么组织各种模块和指令等
-   最好有一个main(boot)文件用来告诉做启动入口，之后相应的模块加载器system(webpack)会按照自己的方式导入到首页index，然后启动
-   `@Component`装饰器下面紧跟的导出类会是一个组件,和名称无关,相当于这个装饰器修饰的是紧跟的类
-    服务导入后不要直接使用而是放在constructor中作为私有变量服务使用,另外如果是获取数据的话,在ngOnInit()中再调用,不要在构造函数中使用
-    建议在中等以上的项目中，每个子功能成单独的模块，即有自己的路由和module,最后同一在app.module中加载，注意使用此种结构别忘了在app.module中还要加载`RouteModule.forRoot()`,特性模块拥有自己的outlet

### 编码规范
- 惯用后缀来描述,*.service,*.pipe等,测试使用spec后缀
- 组件自定义前缀,驼峰命名
- 大写驼峰命名接口和类,接口前不要加I
- 避免为私有属性和方法添加下划线前缀
- 坚持在第三方导入和应用导入之间留一个空行
- 坚持把那些“只用一次”的类收集到CoreModule中，并对外隐藏它们的实现细节。简化的AppModule会导入CoreModule，并且把它作为整个应用的总指挥。
- 坚持使用中线 (dashed) 命名法或烤串 (kebab) 命名法来命名组件中的元素选择器
	```
	@Component(){
	selector:'my-app'
	}
	//wrong
	@Component(){
	selector:'myApp'
	}
	```
- 坚持***自定义组件中***命名事件时，不要带前缀on
- 坚持把表现层逻辑放进组件类中，而不要放在模板里(一般就是计算属性)
- 组件嵌套,如果A组件在它内部使用了组件B,那么不用在A的头部import 组件B,只需要在module启动模块文件头import 组件B,同时声明declaration就行,这样全局都能访问

