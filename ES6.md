### let变量声明以及声明特性

+ 1.变量不能重复声明

  ```javascript
  let person = "Tom";
  let person = "John";
  ```

  

+ 2.块级作用域(全局，函数，eval)

  ```javascript
  {
      let person = "Jerry"
  }
  console.log(person)	//报错 person is not defined
  ```

+ 3.不存在变量提升

  ```javascript
  console.log(cat); //报错 cat is not defined
  let cat = "Tom";
  ```

+ 4.不影响作用域链

  ```javascript
  {
      let num = 1;
      function fn(){
  		console.log(num);
      }
      fn();
  }
  ```

  

### const声明常量以及特点

+ 1.一定要赋初始值

  ```javascript
  const A;
  ```

+ 2.一般常量使用大写

  ```javascript
  const NAME = "Tom";
  ```

+ 3.常量的值不能修改

+ 4.块级作用域

  ```javascript
  {
  	const NAME = "Tom";
  }
  console.log(NAME); //报错 NAME is not defined
  ```

+ 5.对于数组和对象的元素修改，不算做对常量的修改，不会报错

  ```javascript
  const NAMES = ["Tom","Jerry","John"];
  NAMES.push("Lucy");
  Names = 100; //报错
  ```

### 变量的解构赋值

```javascript
//数组解构
const NAMES = ["Tom","Jerry","John","Lucy"];
let [n1,n2,n3,n4] = NAMES;
//对象解构
const PERSON = {
    name:"Tom",
    age:5,
    catchMouse:function(){
        console.log("捉老鼠");
    }
}
let {catchMouse} = PERSON;
catchMouse();
catchMouse();
```

### 模板字符串

```javascript
//1.声明
let str = `模板字符串`;
console.log(str,typeof str);
//2.内容中可以直接出现换行符
let str = `<ul>
			 <li>Tom</li>
			 <li>Jerry</li>
		   </ul>`
//3.变量拼接
let name = "Tom";
let s = `${Tom}捉老鼠`;
```

### 箭头函数

```javascript
//1.定义
let fn = (a,b) =>{
    return a + b;
}
//2.this是静态的，this始终指向函数声明时所在作用域下的this的值
 function getName() {
      console.log(this.name);
    }
    let getName2 = () => {
      console.log(this.name);
    }
    window.name = "Tom";
    const person = {
      name: "Jerry"
    }
    // getName();  Tom
    // getName2(); Tom
    getName.call(person); //Jerry
    getName2.call(person); //Tom
//3.不能作为构造实例化对象
	let Person = (name,age) => {
      this.name = name;
      this.age = age
    }
    let p1 = new Person("Tom",30);
    console.log(p1); //报错 Person不是一个构造函数
//4.不能使用arguments变量
	let fn = () => {
        console.log(arguments); //报错，arguments未定义
    }
    fn(1,2,3);
//5.箭头函数的简写
	//(1)形参有且只有一个时，可以省略小括号
	let add = n => {
        return n + n;
    }
    //(2)当代码体只有一条语句的时候，可以省略花括号,return必须省略
    let pow = n => n*n;
```

### rest参数

```javascript
//ES6引入rest参数，用于获取函数的实参，用来代替arguments
function fn(...args){
    console.log(args)
}
fn("Tom","Jerry","Lucy");
```

### 扩展运算符

```javascript
//	...扩展运算符能将[数组]转换为逗号分隔的 参数序列
const NAMES = ["Tom","Jerry","Lucy"];
function fn(){
	console.log(arguments);
}
fn(...NAMES);
```

### Symbol

```javascript
//创建Symbol
let s = Symbol();
let s2 = Symbol("Name");
let s3 = Symbol("Name");
console.log(s2 === s3)	//false
let s4 = Symbol.for("Name");
let s5 = Symbol.for("Name");
console.log(s4 === s5)	//true
//不能与其他数据类型进行运算

//对象中添加Symbol类型的属性
let game = {}

let methods = {
    up:Symbol(),
    down:Symbol()
};
game[methods.up] = function(){
	console.log("向上")
};
game[methods.down] = function(){
	console.log("向下")
}

let youxi = {
    name:"狼人杀",
    [Symbol("say")]:function(){
		console.log("我可以发言")
    }，  
}
```

### 迭代器

​		迭代器(Iterator)是一种接口，为各种不同的数据结构提供统一的访问机制，任何数据结构只要部署Iterator接口，就可以完成遍历操作

* ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费
* 原生具备iterator接口的数据(可用for of遍历)
  * Array
  * Arguments
  * Set
  * Map
  * String
  * TypedArray
  * NodeList
* 工作原理
  * 创建一个指针对象，指向当前数据结构的起始位置
  * 第一次调用对象的next方法，指针自动指向数据结构的第一个成员
  * 接下来不断调用next方法，指针一直向后移动，直到指向最后一个成员
  * 每调用next方法返回一个包含value和done属性的对象
* 需要自定义遍历数据的时候，要想到迭代器

```javascript
 const Persons = {
      age: 12,
      names: [
        "Tom",
        "Jerry",
        "Lucy",
        "John"
      ],
      [Symbol.iterator]() {
        let index = 0;
        let _this = this;
        return {
          next: function () {
            if (index < _this.names.length) {
              const result = {
                value: _this.names[index],
                done: false
              }
              index++;
              return result;
            } else {
              return {
                value: undefined,
                done: true
              };
            }
          }
        }
      }
    }

    for (let v of Persons) {
      console.log(v);
    }
```

### 生成器

​	生成器函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同

```javascript
function * gen(arg){
    console.log(arg) //AAA
    //yield:函数代码的分隔符
    //代码块1
    let one = yield 111;
    //代码块2
    let two = yield 222;
	//代码块3
    let three = yield 333;
	//代码块4
}
let iterator = gen('AAA');
iterator.next(); //执行 
iterator.next('BBB'); //作为第一个yield语句的返回值
iterator.next('CCC' ); //作为第二个yield语句的返回值
iterator.next('DDD'); //作为第三个yield语句的返回值
```

### 生成器函数实例

​		1s后控制台输出111,2s后控制台输出222,3s后控制台输出333

* 回调地狱

```javascript
setTimeout(() => {
      console.log(111)
      setTimeout(() => {
        console.log(222)
        setTimeout(() => {
          console.log(333)
        }, 3000)
      }, 2000)
    }, 1000)
```

* 生成器函数

``` javascript
 function one() {
      setTimeout(() => {
        console.log(111);
        iterator.next();
      }, 1000)
    }

    function two() {
      setTimeout(() => {
        console.log(222);
        iterator.next()
      }, 2000)
    }

    function three() {
      setTimeout(() => {
        console.log(333);
        iterator.next()
      }, 3000)
    }

    function* gen() {
      yield one();
      yield two();
      yield three();
    }
    let iterator = gen();
    iterator.next();
```

### 生成器函数实例2

​		模拟获取 用户数据 订单数据 商品数据

``` javascript
    function getUsers() {
      setTimeout(() => {
        let data = '用户数据'
        // 第二次调用next方法了，将作为第一个yield语句的返回值
        iterator.next(data);
      }, 1000);
    }

    function getOrders() {
      setTimeout(() => {
        let data = '订单数据'
        iterator.next(data);
      }, 1000);
    }

    function getGoods() {
      setTimeout(() => {
        let data = '商品数据'
        iterator(data);
      }, 1000);
    }

    function* gen() {
      let users = yield getUsers;
      let orders = yield getOrders;
      let goods = yield getGoods;
    }
    let iterator = gen();
    iterator.next()
```



