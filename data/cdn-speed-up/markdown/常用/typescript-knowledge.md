# 所有写法都要用正规写法，禁止花里花俏的写法，否则扑街就怨不了人

### 1.0 TypeScript 对象(比较重要)
```
// 例子1
var object_name = { 
    key1: "value1", // 标量
    key2: "value", 
    key3: function() {
        // 函数
    },
    key4:["content1", "content2"] //集合
}


// 例子2
var sites = {
   site1:"Runoob",
   site2:"Google"
};
// 访问对象的值
console.log(sites.site1);
console.log(sites.site2);


// 例子3 通过对象修改方法
var sites = {
    site1: "Runoob",
    site2: "Google",
    sayHello: function () { } // 类型模板
};
sites.sayHello = function () {
    console.log("hello " + sites.site1);
};
sites.sayHello();


// 例子5 对象作为一个参数传递给函数
var sites = {
    site1:"Runoob",
    site2:"Google",
}; 
var invokesites = function(obj: { site1:string, site2 :string }) {
    console.log("site1 :"+obj.site1);
    console.log("site2 :"+obj.site2);
} 
invokesites(sites);


// 例子6
interface IPoint { 
    x:number,
    y:number 
} 
function addPoints(p1:IPoint,p2:IPoint):IPoint {
    var x = p1.x + p2.x;
    var y = p1.y + p2.y;
    return {x:x,y:y};
}

var newPoint = addPoints({x:3,y:4},{x:5,y:1});
```

### 1.1 TypeScript 基础类型
* `任意类型`    `any`    `声明为 any 的变量可以赋予任意类型的值`

```
let x: any = 1;         // 数字类型
x = 'I am who I am';    // 字符串类型
x = false;              // 布尔类型

let arrayList: any[] = [1, false, 'fine'];
arrayList[1] = 100;
```

* `数字类型`    `number`    `双精度 64 位浮点值。它可以用来表示整数和分数`

```
let binaryLiteral: number = 0b1010;  // 二进制
let octalLiteral: number = 0o744;    // 八进制
let decLiteral: number = 6;          // 十进制
let hexLiteral: number = 0xf00d;     // 十六进制
```

* `布尔类型`    `boolean`    `表示逻辑值：true 和 false`

```
let flag: boolean = true;
```

* `数组类型`    `无`    `声明变量为数组`

```
let arr: number[] = [1, 2];       // 在元素类型后面加上[]
let arr: Array<number> = [1, 2];  // 或者使用数组泛型
```

* `元组`    `无`    `元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同`

```
let x: [string, number];
x = ['Runoob', 1];    // 运行正常
x = [1, 'Runoob'];    // 报错
console.log(x[0]);    // 输出 Runoob
```

* `枚举`    `enum`    `枚举类型用于定义数值集合`

```
// 数字枚举
enum Color {Red, Green, Blue};
let color: Color = Color.Blue;
console.log(color);    // 输出 2
console.log(typeof color')  // 输出number
console.log(typeof color === 'number')  // 输出true

// 字符串枚举，要求每个字段的值都必须是字符串
enum Color {Red='Red', Green='Green', Blue='Blue'};
let color: Color = Color.Blue;
console.log(color);        // 输出字符串 Blue
console.log(typeof color)  // 输出string
console.log(typeof color === 'string')  // 输出true
```

* `void`    `void`    `用于标识方法返回值的类型，表示该方法没有返回值`

```
function hello(): void {
    alert("Hello Runoob");
}
```

* `null`    `null`    `表示对象值缺失`

```
// null是一个只有一个值的特殊类型。表示一个空对象引用
// 用 typeof 检测 null 返回是 object。
console.log(typeof null)  // 输出object
```

* `undefined`     `undefined`    `用于初始化变量为一个未定义的值`

```
// 声明变量的类型，但没有初始值，变量值会设置为 undefined
var variable01:string;    // variable01值是 undefined
var variable02;           // variable02值是 undefined
```

* `never`    `never`    `never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值`

```
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();

// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```

```
// 启用 --strictNullChecks
let x: number;
x = 1;            // 运行正确
x = undefined;    // 运行错误
x = null;         // 运行错误
```

```
// 启用 --strictNullChecks
let x: number | null | undefined;
x = 1;            // 运行正确
x = undefined;    // 运行正确
x = null;         // 运行正确
```

> 注意：TypeScript 和 JavaScript 没有整数类型


### 1.2 变量作用域
```
var global_num = 12          // 全局变量
class Numbers { 
   num_val = 13;             // 实例变量
   static sval = 10;         // 静态变量
   
   storeNum():void { 
      var local_num = 14;    // 局部变量
   } 
} 
console.log("全局变量为: "+global_num)  
console.log(Numbers.sval)   // 静态变量
var obj = new Numbers(); 
console.log("实例变量: "+obj.num_val)
```

### 1.3 TypeScript 循环
```
var j:any; 
var n:any = "a b c"; 
for(j in n) {
    console.log(n[j]);
}


let someArray = [1, "string", false]; 
for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}


// forEach、every 和 some 是 JavaScript 的循环语法，TypeScript 作为 JavaScript 的语法超集，当然默认也是支持的
// 因为 forEach 在 iteration 中是无法返回的，所以可以使用 every 和 some 来取代 forEach
let list = [4, 5, 6];
list.forEach((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
});


let list = [4, 5, 6];
list.every((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
    return true; // Continues
    // Return false will quit the iteration
});

```

### 1.3 函数
```
// 例子1
function add(x: number, y: number): number {
    return x + y;
}
console.log(add(1,2))


// 例子2
function buildName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
}
let result1 = buildName("Bob");                  // 错误，缺少参数
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");         // 正确


// 例子3 将 lastName 设置为可选参数：
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + " " + lastName;
    } else {
        return firstName;
	}
}
let result1 = buildName("Bob");  // 正确
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");  // 正确


// 例子4 默认值
function calculate_discount(price:number,rate:number = 0.50) { 
    var discount = price * rate; 
    console.log("计算结果: ",discount); 
} 
calculate_discount(1000) 
calculate_discount(1000,0.30)


// 例子5 可变参数
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");


// 例子6 可变参数
function addNumbers(...nums:number[]) { 
    var i;
    var sum:number = 0;
    for(i = 0;i<nums.length;i++) {
       sum = sum + nums[i];
    }
    console.log("和为：", sum);
}
addNumbers(1,2,3);
addNumbers(10,10,10,10,10);


// 例子7 匿名函数
var msg = function() {
    return "hello world";
}
console.log(msg());   //调用方式


// 例子8 带参数的匿名函数
var res = function(a:number,b:number) { 
    return a*b;  
}; 
console.log(res(12,2));   //调用方式


// 例子9 匿名函数自调用，匿名函数自调用在函数后使用 () 即可：
(function () {
    var x = "Hello!!";
    console.log(x);
 })();
```

### 1.4 Lambda函数

* Lambda 函数也称之为箭头函数
* 箭头函数表达式的语法比函数表达式更短

```
// 例子1 给某个参数加上10
var foo = (x:number)=>10 + x;  // => 后面单行代码的，自动补上return
console.log(foo(100));         //输出结果为 110


// 例子2 给某个参数加上10
var func = (x)=> { 
    if(typeof x=="number") { 
        console.log(x+" 是一个数字") 
    } else if(typeof x=="string") { 
        console.log(x+" 是一个字符串") 
    }  
} 
func(12) 
func("Tom")


// 例子3 单个参数 () 是可选的
var display = x => {
    console.log("输出为 "+x);
}
display(12);


// 例子4 无参数时可以设置空括号
var disp =()=> { 
    console.log("Function invoked"); 
} 
disp();
```

### 1.5 prototype 实例

* prototype 允许您向对象添加属性和方法

```
function employee(id:number, name:string) { 
    this.id = id;
    this.name = name;
} 
 
var emp = new employee(123,"admin");
employee.prototype.email = "admin@runoob.com";
 
console.log("员工号: "+emp.id);
console.log("员工姓名: "+emp.name); 
console.log("员工邮箱: "+emp.email);
```

### 1.6 初始化数组
```
// 例子1
var sites:string[]; 
sites = ["Google","Runoob","Taobao"];

// 例子2
var sites:string[] = ["Google","Runoob","Taobao"];

// 例子3
var nums:number[] = [1,2,3,4];

// 例子4
var arr:number[] = new Array(4);
for (var i = 0; i<arr.length; i++) { 
    arr[i] = i * 2;
}

// 例子5
var sites:string[] = new Array("Google","Runoob","Taobao","Facebook");
for(var i = 0;i<sites.length;i++) { 
    console.log(sites[i]);
}

// 例子6 多维数组
var multi:number[][] = [[1,2,3],[23,24,25]];
console.log(multi[0][0]);
console.log(multi[0][1]);
console.log(multi[0][2]);
console.log(multi[1][0]);
console.log(multi[1][1]);
console.log(multi[1][2]);

// 例子7
var sites:string[] = new Array("Google","Runoob","Taobao","Facebook");
function disp(arr_sites:string[]) {
    for(var i = 0;i<arr_sites.length;i++) {
        console.log(arr_sites[i]);
    }
}
disp(sites);


// 例子8 作为函数的返回值
function disp():string[] {
    return new Array("Google", "Runoob", "Taobao", "Facebook");
}
var sites:string[] = disp();
for (var i in sites) {
    console.log(sites[i]) 
}
```

### 1.7 数组迭代
```
var j:any; 
var nums:number[] = [1001,1002,1003,1004];
for(j in nums) {
    console.log(nums[j]);
}
```

### 1.8 TypeScript元组，等价于java的Object[]数组

* 元组的元素类型可以不一样的，数组的元素类型是一致的
* push() 向元组添加元素，添加在最后面
* pop() 从元组中移除元素（最后一个），并返回移除的元素

```
// 例子1 插入和移除元素
var mytuple = [10, "Hello", "World", "typeScript"];
mytuple.push(12);                                    // 添加到元组中
var item = mytuple.pop();

// 例子2 通过下标修改取值
var mytuple = [10, "Runoob", "Taobao", "Google"]; // 创建一个元组
mytuple[0] = 121;    // 更新元组元素

// 例子3 解构元组，我们也可以把元组元素赋值给变量
var a =[10, "Runoob"];
var [b,c] = a;   // 等价于 var b=a[0], c=a[1];
```


### 1.9 TypeScript 联合类型

* 可以将变量赋予事先申明的多种类型，不能赋予事先没申明的数据类型

```
// 例子1
var val:string|number;
val = 12;
val = "Runoob";

// 例子2
var arr:number[]|string[];
var i:number;
arr = [1,2,4];

for(i = 0;i<arr.length;i++) {
   console.log(arr[i]);
}

arr = ["Runoob","Google","Taobao"];

for(i = 0;i<arr.length;i++) {
   console.log(arr[i]);
}

// 例子3 也可以将联合类型作为函数参数使用
function disp(name:string|string[]) {
    if(typeof name == "string") {
        console.log(name);
    } else {
        var i;
        for(i = 0;i<name.length;i++) {
            console.log(name[i]);
        }
}
}
disp("Runoob");
console.log("输出数组....");
disp(["Runoob","Google","Taobao","Facebook"]);
```

### 1.10 接口
```
// 例子1
interface IPerson {
    firstName:string,
    lastName:string,
    sayHi: ()=>string
}

var customer:IPerson = {
    firstName:"Tom",
    lastName:"Hanks",
    sayHi: ():string =>{return "Hi there"}
} 

console.log(customer.firstName);
console.log(customer.lastName);
console.log(customer.sayHi());

var employee:IPerson = {
    firstName:"Jim",
    lastName:"Blakes",
    sayHi: ():string =>{return "Hello!!!"}
}

console.log(employee.firstName);
console.log(employee.lastName);



// 例子2
interface RunOptions {
    program:string;
    commandline:string[]|string|(()=>string);
}

var options:RunOptions = {program:"test1",commandline:"Hello"};
console.log(options.commandline);

options = {program:"test1",commandline:["Hello","World"]};
console.log(options.commandline[0]);

options = {program:"test1",commandline:()=>{return "**Hello World**";}};
var fn:any = options.commandline;



// 例子3 单继承实例
interface Person {
   age:number
}

interface Musician extends Person {
   instrument:string
}

var drummer = <Musician>{}; 
drummer.age = 27;
drummer.instrument = "Drums";
console.log("年龄:  "+drummer.age);
console.log("喜欢的乐器:  "+drummer.instrument);


// 例子4 多继承实例
interface IParent1 {
    v1:number
}

interface IParent2 {
    v2:number
}

interface Child extends IParent1, IParent2 { };
var Iobj:Child = { v1:12, v2:23};
console.log("value 1: "+Iobj.v1+" value 2: "+Iobj.v2);
```


### 1.11 类
```
// 例子1
class Car {
    engine:string; // 字段

    constructor(engine:string) {  // 构造函数
        this.engine = engine;
    }
    
    disp():void {  // 方法
        console.log("发动机为 :   "+this.engine);
    }
}
var obj = new Car("Engine 1");
console.log("读取发动机型号 :  "+obj.engine);
obj.disp();

// 例子2 继承
class Shape {
    Area:number;

    constructor(a:number) {
        this.Area = a;
    }
}

class Circle extends Shape {
    disp():void {
        console.log("圆的面积:  "+this.Area);
    }
}

var obj = new Circle(223);
obj.disp();


// 例子3 多重继承
class Root { 
    str:string; 
} 
 
class Child extends Root {} 
class Leaf extends Child {} // 多重继承，继承了 Child 和 Root 类

var obj = new Leaf(); 
obj.str ="hello";
console.log(obj.str);


// 例子4 继承类的方法重写
class PrinterClass {
   doPrint():void {
      console.log("父类的 doPrint() 方法。");
   }
}

class StringPrinter extends PrinterClass {
   doPrint():void {
      super.doPrint(); // 调用父类的函数
      console.log("子类的 doPrint()方法。");
   }
}


// 例子5 static 关键字
class StaticMem {
   static num:number;

   static disp():void {
      console.log("num 值为 "+ StaticMem.num);
   }
}

StaticMem.num = 12;     // 初始化静态变量
StaticMem.disp();       // 调用静态方法

// 例子6 instanceof 运算符 instanceof 运算符用于判断对象是否是指定的类型，如果是返回 true，否则返回 false
class Person{ }
var obj = new Person();
var isPerson = obj instanceof Person;
console.log("obj 对象是 Person 类实例化来的吗？ " + isPerson);


// 例子7 类和接口
interface ILoan {
   interest:number
}
 
class AgriLoan implements ILoan {
   interest:number;
   rebate:number;

   constructor(interest:number,rebate:number) {
      this.interest = interest
      this.rebate = rebate
   }
}

var obj = new AgriLoan(10, 1);
console.log("利润为 : "+obj.interest+"，抽成为 : "+obj.rebate );
```


### 1.11 TypeScript 模块

* 导出模块让其他地方用，模块导出使用 export 关键字
* 模块导入使用 import 关键字

```
// 例子1
// IShape.ts 文件
export interface IShape {
   draw();
}

// Circle.ts 文件
import shape = require("./IShape");
export class Circle implements shape.IShape {
   public draw() {
      console.log("Cirlce is drawn (external module)");
   }
}

// Triangle.ts 文件
import shape = require("./IShape");
export class Triangle implements shape.IShape {
   public draw() {
      console.log("Triangle is drawn (external module)");
   }
}

// TestShape.ts 文件
import shape = require("./IShape");
import circle = require("./Circle");
import triangle = require("./Triangle");

function drawAllShapes(shapeToDraw: shape.IShape) {
   shapeToDraw.draw();
}

drawAllShapes(new circle.Circle());
drawAllShapes(new triangle.Triangle());
```

### 1.12 TypeScript 声明文件

* 声明文件以 .d.ts 为后缀
* 开发过程中不可避免要引用其他第三方的 JavaScript 的库。虽然通过直接引用可以调用库的类和方法，但是却无法使用TypeScript 诸如类型检查等特性功能。
* 为了解决这个问题，需要将这些库的函数和方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。
* 通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了

```
// 引入jquery
import $ = require("jquery");
$.each(['a','b','c'], function (index, value) {
    console.log(index + " " + value);
})
console.log($('#greeting').html());
```
