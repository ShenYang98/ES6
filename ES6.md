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

### Promise

​		promise是ES6引入的异步编程的新解决方案，语法上Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果

* Promise构造函数：Promise(excutor){}
* Promise.prototype.then方法
* Promise.prototype.catch方法

```javascript
 const p = new Promise((resolve, reject) => {
      setTimeout(() => {
        let data = '数据';
        let err = '失败';
        // resolve(data)
        reject(err)
      }, 1000);
    })
    p.then(res => {
      console.log(res);
    }, err => {
      console.log(err);
    })
```

### Promise封装读取文件

```javascript
const fs = require("fs");
fs.readFile("test.html", (err, data) => {
  if (err) {
    console.log(err)
  }
  console.log(data.toString());
})

const p = new Promise((resolve, reject) => {
  fs.readFile("test.html", (err, data) => {
    if (err) {
      reject(err)
    }
    resolve(data);
  })
})

p.then((res) => {
  console.log(res.toString());
}, (err) => {
  console.log("读取失败");
})
```

## AJAX

### 1.AJAX的介绍

+ AJAX全称为Asynchronous JavaScript And XML，就是异步的JS和XML，通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**
+ **优点**：
  + 可以无需刷新页面与服务器端进行通信
  + 允许根据用户事件来更新部分页面内容
+ **缺点**：
  + 没有浏览历史，不能回退
  + 存在跨域问题(同源)
  + SEO不友好

### 2.HTTP协议

+ 请求报文

```javascript
行		POST /s?ie=utf-8 HTTP/1.1
头		Host: atguigu.com
		Cookie: name=guigu
		Content-type: application/x-www-form-urlencoded
		User-Agent: chrome 83
空行
体		username=admin&password=admin
```

+ 响应报文

```javascript
行		HTTP/1.1 200 OK
头		Content-Type: text/html;charset=utf-8
		Content-length: 2048
		Content-encoding: gzip
空行
体		<html>
    		<head>
    		</head>
			<body>
    			<h1>响应体</h1>
    		</body>
    	</html>

403：没有权限，被禁止
401：未授权
500：内部错误
```

### 3.express

```javascript
// 1.引入express
const express = require("express");

// 2.创建应用对象
const app = express();

//3.创建路由规则
// request是对请求报文的封装
// response是对响应报文的封装
app.get("/", (request, response) => {
  // 设置响应
  response.send("hello express")
})

//4.监听端口启动服务
app.listen(8000, () => {
  console.log("服务已经启动,8000端口监听中")
})
```

### 4.AJAX准备

```javascript
// 1.引入express
const express = require("express");

// 2.创建应用对象
const app = express();

//3.创建路由规则
// request是对请求报文的封装
// response是对响应报文的封装
app.get("/server", (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader("Access-Control-Allow-Origin","*")
  // 设置响应体
  response.send("hello AJAX")
})

//4.监听端口启动服务
app.listen(8000, () => {
  console.log("服务已经启动,8000端口监听中")
})
```

### 5.GET请求

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px #90b;
    }
  </style>
</head>

<body>
  <button>点击发送请求</button>
  <div id="result"></div>

  <script>
    // 获取button元素
    const btn = document.getElementsByTagName("button")[0];
    const result = document.getElementById("result");
    // 绑定事件
    btn.onclick = function () {
      // 1.创建对象
      const xhr = new XMLHttpRequest();
      // 2.初始化 设置请求方法和url
      xhr.open("GET", "http://127.0.0.1:8000/server?a=100&b=200&c=300");
      // 3.发送
      xhr.send();
      // 4.事件绑定 处理服务端返回的结果
      // readystate是xhr对象中的属性，表示状态 0,1,2,3,4
      // 3表示部分结果返回，4表示全部结果返回
      xhr.onreadystatechange = function () {
        // 判断 (服务端反悔了所有的结果)
        if (xhr.readyState === 4) {
          // 判断响应状态码
          // 2xx 成功
          if (xhr.status >= 200 && xhr.status < 300) {
            // 处理结果 包括行 头 空行 体
            // 1.响应行
            console.log(xhr.status); //状态码
            console.log(xhr.statusText) //状态字符串
            console.log(xhr.getAllResponseHeaders()) //所有响应头
            console.log(xhr.response) //响应体
            // 设置result的文本
            result.innerHTML = xhr.response;
          } else {

          }
        }
      }
    }
  </script>
</body>

</html>
```

### 6.POST请求

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px #903;
    }
  </style>
</head>

<body>
  <div id="result"></div>
  <script>
    // 获取元素对象
    const result = document.getElementById("result");
    // 绑定事件
    result.addEventListener("mouseover", function () {
      // 1.创建对象
      const xhr = new XMLHttpRequest();
      // 2.初始化 设置类型和url
      xhr.open("Post", "http://127.0.0.1:8000/server");
      // 设置请求头信息   请求体内容的类型 参数查询字符串了类型
      xhr.setRequestHeader("Content-T/*  */ype", "application/x-www-form-urlencoded")
      // 也可以自定义 但是要加上一个特殊的响应头
      xhr.setRequestHeader("name", "shenyang");
      // 3.发送 参数查询字符串
      xhr.send("a:100&b:200&c:300");
      // 4.事件绑定
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            result.innerHTML = xhr.response;
          }
        }
      }
    })
  </script>
</body>

</html>
```

```javascript
// 1.引入express
const express = require("express");

// 2.创建应用对象
const app = express();

//3.创建路由规则
// request是对请求报文的封装
// response是对响应报文的封装
app.get("/server", (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader("Access-Control-Allow-Origin", "*")
  // 设置响应体
  response.send("hello AJAX")
})

// 可以接收任意类型的请求：all
app.all("/server", (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader("Access-Control-Allow-Origin", "*");
  // 响应头
  response.setHeader("Access-Control-Allow-Headers", "*");
  // 设置响应体
  response.send("hello AJAX")
})

//4.监听端口启动服务
app.listen(8000, () => {
  console.log("服务已经启动,8000端口监听中")
})
```

### JSON数据

```javascript
 <script>
    const result = document.getElementById("result");
    // 绑定键盘按下事件
    window.onkeydown = function () {
      // 发送请求
      const xhr = new XMLHttpRequest();
      // 设置响应体数据的类型
      // xhr.responseType = "json";
      // 初始化
      xhr.open("GET", "http://127.0.0.1:8000/json-server");
      // 发送
      xhr.send();
      // 事件绑定
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            // 手动对数据转化
            let data = JSON.parse(xhr.response);
            result.innerHTML = data.name;
            // 自动转换,设置响应体数据类型
          }
        }
      }
    }
  </script>
```

```javascript
app.all("/json-server", (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader("Access-Control-Allow-Origin", "*");
  // 响应头
  response.setHeader("Access-Control-Allow-Headers", "*");
  const data = {
    name: "Tom"
  }
  const str = JSON.stringify(data);
  // 设置响应体
  response.send(str)
})
```

### AJAX在ie浏览器中有缓存

```javascript
// 在路径后加时间戳
xhr.open("GET","http://127.0.0.1:8000/ie?t=" + Date.now());
```

### 请求超时与网络异常

```javascript
  <script>
    const result = document.getElementById("result")
    result.addEventListener("click", function () {
      const xhr = new XMLHttpRequest();
      // 超时设置,两秒还没有请求返回 则取消
      xhr.timeout = 2000;
      // 超时回调
      xhr.ontimeout = function () {
        alert("网络异常，请稍后重试！")
      }
      // 网络异常回调
      xhr.onerror = function () {
        alert("网络异常")
      }
      xhr.open("Post", "http://127.0.0.1:8000/delay");
      xhr.send();
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            result.innerHTML = xhr.response;
          }
        }
      }
    })
  </script>
```

```javascript
app.all("/delay", (request, response) => {
  // 设置响应头，设置允许跨域
  response.setHeader("Access-Control-Allow-Origin", "*");
  setTimeout(() => {
    response.send("延时响应");
  }, 3000);
})
```

### AJAX取消请求

```javascript
  <button>点击发送</button>
  <button>点击取消</button>
  <script>
    const btns = document.querySelectorAll("button");
    let x = null;
    btns[0].onclick = function () {
      x = new XMLHttpRequest();
      x.open("GET", "http://127.0.0.1:8000/delay");
      x.send();
    }

    btns[1].onclick = function () {
      x.abort();
    }
  </script>
```

### AJAX请求重复发送问题

```javascript
<body>
  <button>点击发送</button>
  <button>点击取消</button>
  <script>
    const btns = document.querySelectorAll("button");
    let x = null;
    // 标识变量 是否正在发送AJAX请求
    let isSending = false;
    btns[0].onclick = function () {
      // 判断标识变量 如果正在发送，则取消该请求，创建一个新的请求
      if (isSending) {
        x.abort()
      };
      x = new XMLHttpRequest();
      // 修改标识变量的值
      isSending = true;
      x.open("GET", "http://127.0.0.1:8000/delay");
      x.send();
      x.onreadystatechange = function () {
        if (x.readyState === 4) {
          // 修改标识变量
          isSending = false;
        }
      }
    }

    btns[1].onclick = function () {
      x.abort();
    }
  </script>
```

### axios发送AJAX请求

```javascript
<button>GET</button>
  <button>POST</button>
  <button>AJAX</button>
  <script>
    const btns = document.querySelectorAll("button");

    btns[0].onclick = function () {
      // GET请求
      axios.get("http://127.0.0.1:8000/server", {
        // url参数
        params: {
          id: 100,
          vip: 7
        },
        // 请求头信息
        headers: {
          name: "Tom",
          age: 20
        }
      }).then(value => {
        console.log(value);
      });
    }
  </script>
```

```javascript
    btns[2].onclick = function () {
      axios({
        //请求方法
        method: "POST",
        // url
        url: "/server",
        // url参数
        params: {
          vip: 10,
          level: 30
        },
        // 头信息
        headers: {
          a: 100,
          b: 200
        },
        // 请求体参数
        data: {
          username: "admin",
          password: "admin"
        }
      }).then(response => {
        // 响应状态码
        console.log(response.status);
        // 响应状态字符串
        console.log(response.statusText);
        // 响应头信息
        console.log(response.headers);
        // 响应体
        console.log(response.data);
      })
    }
  </script>
```



### Promise封装AJAX请求

