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







