title: TypeScript新手注意
date: 2016-12-04 04:36:02
tags: [TypeScript, 学习, 注意事项]
---

### 语法

- `let x:any` `let x:object`都可以赋值任意类型，不同在于， Object类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法：
- `never` 表示那些永远不存在的值得类型
<!--more-->
- 类型断言`()<string>name)`或`(name as string)`
- 接口类型检查，只要传入的变量包含接口定义的属性就行，另外和属性顺序无关
- 可选属性

	```
	interface Pserson
	{
	name?:string;
	age:number;
	}
	```
- 只读属性


	```
	interface Point 
	{
    readonly x: number;
    readonly y: number;
	}
	```
- 接口定义，类实现,注意关键字`implements`

	```
	interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
	}

	class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
	}
	
	```
- 扩展接口,注意关键字`extends`

	```
	interface Shape {
    color: string;
	}

	interface PenStroke {
    penWidth: number;
	}

	interface Square extends Shape, PenStroke {
    sideLength: number;
	}

	let square = <Square>{};
	square.color = "blue";
	square.sideLength = 10;
	square.penWidth = 5.0;
	```
- 限定符，public,private,protected（类，和子类可访问）,readonly
- 存取器，get,set,static(属性，方法都可直接添加了不像es6只能方法添加)
- 抽象类和接口对比，两者都是只定义签名不包含实现。
   不同在于前者需要abstract关键字修饰，同时抽象***类***可以包含具体方法或抽象方法，抽象***方法***必须子类实现，接口不能有实现
   
   ```
   //接口
   interface Person
   {
     name:string;
     getName();string;//不能用方法体
   }
   ```
- 泛型，泛型约束

	```
	interface Lengthwise {
    length: number;
	}

	function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no 	more error
    return arg;
	}
	//类似于c#里的where T :base
	
	```
- 联合类型
	
	```
	function test(name:string|number)//是两种类型之一，但调用name方法时只能访问两种类型共有的方法，否则得加判断
	function padLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
	}
	
	```
	
- 模块导入导出
	***类和函数声明***可以直接被默认导出，接口不能默认导出，但可以导出
- 声明合并，接口合并，命名空间合并

### 规范
- 重载方法不要写多多个每个参数不同这种写法，应该写一个，然后使用可选参数形式

	```
	/* 错误 */
	interface Example {
    diff(one: string): number;
    diff(one: string, two: string): number;
    diff(one: string, two: string, three: boolean): number;
	}
	/* OK */
	interface Example {
    diff(one: string, two?: string, three?: boolean): number;
	}
	```
- 使用联合类型

	不要为仅在某个位置上的参数类型不同的情况下定义重载：


	```
	/* WRONG */
	interface Moment {
    utcOffset(): number;
    utcOffset(b: number): Moment;
    utcOffset(b: string): Moment;
	}
	/* OK */
	interface Moment {
    utcOffset(): number;
    utcOffset(b: number|string): Moment;
	}
	```

	