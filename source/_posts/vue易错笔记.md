title: vue易错笔记.md
date: 2016-11-18 00:23:01
tags: [前端, 学习, html, js, css, vue]
---

### 几点要点

- 没有控制器的概念，所有的方法和逻辑应该写在方法中或者生命周期方法中

- {{x}}插值（mustache）不能在属性中使用，只能在模板中使用，属性中应该用`v-bind:title="xxx"`这样，不能`title={{xxx}}`

- 绑定都只能包含单个表达式，所以下面的例子都不会生效。


<!--more-->


```
	<!-- 这是语句，不是表达式 -->
	{{ var a = 1 }}
	<!-- 流控制也不会生效，请使用三元表达式 -->
	{{ if (ok) { return message } }}

```

- v-bind:title 缩写:title,v-on:click= 缩写@click=

- 计算属性computed和watch,耗时长用watch,另外计算属性是依赖基础属性的如果基础属性没变化是不会调用的，如下

```
	computed: {
	  now: function () {
	    return Date.now()
	  }
	} //此时now就不会更新，显然这时候使用method更好
    
```

- 组件中ｄａｔａ必须是方法而不能是对象，这样当有多个组件时，不会出现都引用同一个的问题

- 组件实例化要放在根实例化之前

- v-model可以添加修饰值．lazy,number,trim

- props,父组件给子ｐrops传值时，使用v-bind `<my-cop ：my-prop='1'></mycop>` 而不是直接`<my-cop my-prop='1'></mycop>` ，后者传递的只是一个字符１，不是数字

- 父组件事件监听和react差不多，需要注意的是如果要监听原生事件添加.native修饰符，如`<my-component v-on:click.native="doTheThing"></my-component>`

- slot内容分发，类似angular 的transclusion,有匿名和具名两种，组合组件时常用

- 多个组件可以使用同一个挂载点，然后动态地在它们之间切换。使用 *** 保留的< component>元素 ***，动态地绑定到它的is特性：

```
    var vm = new Vue({
    el: '#example',
    data: {
    	currentView: 'home'
        },
        components: {
    	home: { /* ... */ },
    	posts: { /* ... */ },
    	archive: { /* ... */ }
        }
    })；

```


```
	<component v-bind:is="currentView">
	  <!-- 组件在 vm.currentview 变化时改变！ -->
	</component> //通过改变currentView的值来动态改变组件内容

```

- ref引用，类似react　需要注意的是ref 被用来给元素或子组件注册引用信息。引用信息会根据父组件的 $refs 对象进行注册。如果在普通的DOM元素上使用，引用信息就是元素; 如果用在子组件上，引用信息就是组件实例:


```
	<!-- vm.$refs.p will the DOM node -->
	<p ref="p">hello</p>
	<!-- vm.$refs.child will be the child comp instance -->
	<child-comp ref="child"></child-comp>

```

- v-once 渲染结果缓存

```
	Vue.component('terms-of-service', {
		  template: '\
		    <div v-once>\
		      <h1>Terms of Service</h1>\
		      ... a lot of static content ...\
		    </div>\
		  '
		})
```




	