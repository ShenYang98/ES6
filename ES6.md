### let变量声明以及声明特性

+ 1.变量不能重复声明

  ```javascript
  let person = "Tom";
  let person = "John;
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


+ 使用let关键字声明的变量具有暂时性死区特性

  ```javascript
      var num = 10;
      if (true) {
        console.log(num); //报错，不会寻找外部的num
        let num = 20;
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
//2.this是静态的，this始终指向函数声明时所在作用域下的this的值，也就是指向函数定义位置的上下文this
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

+ 面试题：

```javascript
    // var age = 100;
    var obj = {
      age: 20,
      say: () => {
        console.log(this.age) //undefined，obj是对象，没有作用域，因此箭头函数定义在全局作用域上，this指向的是window
      }
    }
    obj.say();
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

​		promise是ES6引入的异步编程的新解决方案，语法上Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果，**支持链式调用，可以解决回调地狱问题**

* Promise构造函数：Promise(excutor){}
* Promise.prototype.then方法
* Promise.prototype.catch方法
* resolve可以将promise对象的状态设置为”成功“，reject可以将promise对象的状态设置为“失败”

```javascript
 const p = new Promise((resolve, reject) => {
      setTimeout(() => {
        let data = '数据';
        let err = '失败';
        // resolve(data)
        reject(err)
      }, 1000);
    })
    p.then(value => {
      console.log(value);
    }, reason => {
      console.log(reason);
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

p.then((value) => {
  console.log(value.toString());
}, (reason) => {
  console.log("读取失败");
})
```

```javascript
function mineReadFile(path) {
  return new Promise((resolve, reject) => {
    require('fs').readFile(path, (err, data) => {
      if (err) {
        reject(err)
      }
      resolve(data)
    })
  })
}

mineReadFile("./async.js").then(value => {
  console.log(value.toString());
}, reason => {
  console.log("读取失败")
})
```

### util.promisify

+ 传入一个遵循常见的错误优先的回调风格的函数（即以(err,value) => ... 回调作为最后一个参数），并返回一个返回promise的版本

```javascript
const util = require("util");
const fs = require("fs");

let myReadFile = util.promisify(fs.readFile);
myReadFile("./async.js").then(value => {
  console.log(value);
})

```

### Promise封装AJAX请求

```javascript
    const btn = document.querySelector(".btn");
    btn.addEventListener("click", function () {
      const xhr = new XMLHttpRequest();
      xhr.open("GET", "https://api.apiopen.top/getJoke");
      xhr.send();
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            console.log(xhr.response);
          } else {
            console.log(xhr.status)
          }
        }
      }
    })
```

```javascript
    function sendAJAX(url) {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open("GET", url);
        xhr.send();
        xhr.onreadystatechange = function () {
          if (xhr.readyState === 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
              resolve(xhr.response);
            } else {
              reject(xhr.status);
            }
          }
        }
      })
    }
    sendAJAX('https://api.apiopen.top/getJoke').then(value => {
      console.log(value);
    })
```

### Promise对象状态属性介绍

+ 状态指的是实例对象中的一个属性[PromiseState]
+ 有三种状态：
  + pending：未决定的 
  + resolved / fullfilled：成功
  + rejected：失败 
+ 只能由pending变为成功或失败，不能由成功转为失败或由失败转为成功，并且只能改变一次

### Promsie对象结果值属性介绍

+ 实例对象中的另一个属性[PromiseResult]
+ 保存着异步任务成功/失败的结果(err,data)

### Promise API

#### 构造器

+ Promise(excutor){}：executor Promise内部立即同步调用，异步操作在执行器中执行

#### Promise.prototype.catch

+ (onRejected) => {}：指定失败的回调函数(reason) => {}

#### Promise.resolve方法：

+ (value) => {}
+ value：成功的数据或promise对象
+ 说明：返回一个失败的promsie对象

```javascript
let p1 = Promise.resolve(521);
//如果传入的参数为非promise类型的对象，则返回的结果为成功promise对象
//如果传入的参数为Promise对象，则参数的结果决定了resolve的结果
console.log(p1);  
let p2 = Promise.resolve(new Promise((resolve,reject) => {
	resolve("OK");
   //reject("Error"); 
}))
//reject要有对应的回调处理，可以用catch处理
p2.catch(reason => {
    console.log(reason);
})
```

#### Promise.reject方法：

+ (reason) => {}
+ reason:失败的原因
+ 说明：返回一个失败的promise对象

```javascript
let p = Promise.reject("521");
let p2 = Promise.reject("John");
let p3 = Promise.reject(new Promise((resolve, reject) => {
      resolve("123");
}))
//返回的结果永远是失败的，传入的是什么，失败的结果就是什么
console.log(p2);
```

#### Promise.all方法：

+ (promises) => {}
+ promises：包含n个promise的数组
+ 说明：返回一个新的promise，只有所有的promise都成功才成功，只要有一个失败了就直接失败

```javascript
let p1 = new Promise((resolve, reject) => {
  resolve("成功")
})
// let p2 = Promise.resolve("OK");
let p2 = Promise.reject("Error");
const result = Promise.all([p1, p2]);
console.log(result); //状态：rejected 值：Error
```

#### Promise.race方法：

+ (promises) => {}
+ promises包含n个promise的数组
+ 说明：返回一个新的promsie，第一个**完成**的promise的结果状态就是最终的结果状态

```javascript
let p1 = new Promise((resolve, reject) => {
setTimeout(() => {
    resolve("成功");
  }, 2000);
})
// let p2 = Promise.resolve("OK");
let p2 = Promise.reject("Error");
const result = Promise.race([p1, p2]);
console.log(result)
```

#### Promise的关键问题

+ 如何改变promise的状态

  + resolve(value)：pending转为resolved
  + reject(reason)：pending转为rejected
  + 抛出异常：pending转为rejected

+ 一个promise指定多个成功/失败回调函数，都会调用吗？

  + 当promise改变为对应状态时都会调用

  ```javascript
  let p = new promise((resolve,reject) => {
  	resolve("OK");
  })
  p.then(value => {
      console.log(value);
  })
  p.then(value => {
      alert(value);
  })
  ```

+ 改变状态与指定回调谁先谁后

  + 都有可能，正常情况下是先指定回调再改变状态，但也可以先改状态再指定回调
  + 如果是异步任务，就是先改变状态再指定回调
  + 如何先改状态再指定回调？
    + 在执行器中直接调用resolve()/reject()
    + 延迟更长时间才调用then()
  + 什么时候才能拿到数据
    + 如果先指定的回调(异步任务)，那当状态发生改变时，回调函数就会调用，得到数据
    + 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据

+ then方法返回的promise结果由什么决定

  + 简单表达：由then()指定的回调函数执行的结果决定

  + 详细表达：

    + 如果抛出异常，新promise变为rejected，reason为抛出的异常
    + 如果返回的是非promise的任意值，新promise变为resolved，value为返回的值
    + 如果返回的是另一个新promise，此promise的结果就会成为新promise的结果

    ```javascript
    let p = new Promise((resolve,reject) => {
    	resolve("ok");
    });
    
    let result = p.then(value => {
        //console.log(value);
        //throw "出了问题"
        //return 123;
        return new Promise((resolve,reject) => {
    		resolve("success");
        });
    },reason =>{
        console.warn(reason);
    })
    ```

+ promise如何串连多个操作任务

  + promise的then()返回一个新的promise，可以形成then()的链式调用
  + 通过then的链式调用串连多个同步/异步任务

  ```javascript
      let p1 = new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve("OK");
        }, 1000);
      });
      p1.then(value => {
        return new Promise((resolve, reject) => {
          resolve("success");
        })
      }).then(value => {
        console.log(value) //输出success
      }).then(value => {
        console.log(value) //输出undefined，因为前一个then的返回结果不是一个promise对象
      })
  ```

+ promise异常穿透

  + 当使用promise的then链式调用时，可以在最后指定失败的回调
  + 前面任何操作出了异常，都会传到最后失败的回调中处理

  ```javascript
      let p1 = new Promise((resolve, reject) => {
        // resolve("ok");
        reject("err")
      })
      p1.then(value => {
        console.log(111);
      }).then(value => {
        console.log(222);
      }).catch(reason => {
        console.warn(reason)
      })
  ```

+ 中断promise链

  + 当时用promise的then链式调用时，在中间中断，不在调用后面的回调函数
  + 办法：在回调函数中返回一个pendding状态的promise对象(throw或者返回一个rejected的Promise，后面then方法的回调都不会执行，返回一个pendding状态的Promise，包括catch里的回调也不会执行)

  ```javascript
      let p1 = new Promise((resolve, reject) => {
        // resolve("ok");
        reject("err")
      })
      p1.then(value => {
        console.log(111);
        //有且只有一个方式，返回一个pendding状态的promise对象
        return new Promise(() => {});l
      }).then(value => {
        console.log(222);
      }).catch(reason => {
        console.warn(reason)
      })
  ```


#### async和await结合

+ 读取文件

     ```javascript
      // 回调函数
      const fs = require("fs");
      const util = require("util");
      const mineReadFile = util.promisify(fs.readFile());
      fs.readFile("1.html", (err, data1) => {
        if (err) throw err;
        fs.readFile("2.html", (err, data2) => {
          if (err) throw err;
          fs.readFile("3.html", (err, data3) => {
            if (err) throw err;
            console.log(data1 + data2 + data3);
          })
        })
      })
     ```

  ```javascript
      // async+await
      async function main() {
        try {
          let data1 = await mineReadFile("1.html");
          let data2 = await mineReadFile("2.html");
          let data3 = await mineReadFile("3.html");
          console.log(data1 + data2 + data3);
        } catch (e) {
          console.log(e);
        }
      }
      main();
  ```


### 宏队列和微队列

+ JS中用来存储待执行回调函数的队列包含2个不同特定的队列

+ 宏队列：用来保存待执行的宏任务，比如dom事件回调	ajax回调	定时器回调

+ 微队列：用来保存待执行的微任务，比如promise回调     mutationObserver回调

  + JS引擎首先先执行完所有的同步代码，再执行队列里的回调函数

  + 每次准备取出第一个宏任务执行前，都要将所有的微任务一个一个取出来执行

+ 面试题：

  + 1

  ```javascript
      setTimeout(() => {
        console.log(1)
      }, 0);
      Promise.resolve().then(() => {
        console.log(2)
      })
      Promise.resolve().then(() => {
        console.log(4)
      })
      console.log(3)
  	//3 2 4 1
  ```

  + 2

  ```javascript
      setTimeout(() => {
        console.log(1)
      }, 0)
      new Promise((resolve) => {
        console.log(2) //执行器函数是同步执行的
        resolve()
      }).then(() => {
        console.log(3)
      }).then(() => {
        console.log(4)
      })
      console.log(5)
      // 2 5 3 4 1
      // 2执行后3的状态修改为成功，3立即放入微队列
      // 但是在3还未执行，4的then没有放入微任务队列，因为3的回调函数没执行，4的状态没有修改
      // 取出微队列的3后，3执行后，4的状态修改为成功，再把4放入微任务
  ```

  + 3

  ```javascript
      const first = () => (
        new Promise((resolve, reject) => {
          console.log(3)
          let p = new Promise((resolve, reject) => {
            console.log(7);
            setTimeout(() => {
              console.log(5)
              resolve(6)
            }, 0);
            resolve(1)
          })
          resolve(2)
          p.then((arg) => {
            console.log(arg);
          })
        })
      )
      first().then((arg) => {
        console.log(arg);
      })
      console.log(4)
      // 3 7 4 1 2 5
      // 首先寻找同步执行，first在4之前执行，先输出3，7是执行器函数，再输出7，最后输出同步执行的4
      // resolve(1)立即执行，p的状态被修改为成功，然后执行resolve(2),前一个Promise的状态也被修改为成功
      // 但是p的回调先执行，将1放入微队列，然后执行first的回调，将2放入微队列
  	// 定时器中的resolve(6)不会再打印，因为p的状态已经修改了一次，变为了成功
  ```

  + 4

  ```javascript
      setTimeout(() => {
        console.log(0)
      }, 0);
      new Promise((resolve, reject) => {
        console.log(1)
        resolve()
      }).then(() => {
        console.log(2)
        new Promise((resolve, reject) => {
          console.log(3)
          resolve()
        }).then(() => {
          console.log(4)
        }).then(() => {
          console.log(5)
        }).then(() => {
          console.log(6)
        })
      })
  
      new Promise((resolve, reject) => {
        console.log(7)
        resolve()
      }).then(() => {
        console.log(8)
      })
      // 首先将0放入宏队列，执行器函数1同步执行，输出1，然后修改回调函数状态为成功，将2放入微队列
      // 6的回调是立即执行的，但是上面的回调还没执行，不会放入微队列
      // 第二个Promise先同步输出7，然后将8的回调函数修改为成功，将8放入微队列
      // 然后输出2的回调函数，2中Promise先输出3，然后修改状态为成功，将4放入微队列，5被缓存，不会放入队列，此时4在微队列中还未执行，6的回调放入微队列
      // 1 7 2 3 
      // 宏：[0]
      // 微：[8,4,6]
      // 取出8执行，再取出4执行，此时5放入微队列
      // 有时后面then的回调要等到之前then回调执行才能放入队列
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

### fetch函数发送AJAX请求

```javascript
  <button>AJAX请求</button>
  <script>
    const btn = document.querySelector("button");
    btn.onclick = function () {
      fetch("http://127.0.0.1:8000/server?vip=10", {
        // 请求方法
        method: "POST",
        // 请求头
        headers: {
          name: "Tom"
        },
        // 请求体
        body: "username=admin&password=admin"
      }).then(response => {
        return response.text();
        // return response.json();
      }).then(response => {
        console.log(response);
      })
    }
  </script>
```

### Promise封装AJAX请求

```javascript
  <script>
    const xhr = new XMLHttpRequest();
    const p = new Promise((resolve, reject) => {
      xhr.open("GET", "https://api.apiopen.top/getJoke");
      xhr.send();
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            resolve(xhr.response);
          } else {
            reject(xhr.status);
          }
        }
      }
    })

    p.then((res) => {
      console.log(res);
    }, (err) => {
      console.log(err);
    })
  </script>
```

## async和await

​	async和await可以让异步代码像同步代码一样

### async函数

+ async函数的返回值为promise对象

+ promise对象的结果由async函数执行的返回值决定

  + 如果返回值是一个非Promise类型的数据，结果就是一个成功的promise对象
  + 如果返回值是一个成功的Promise对象，结果就是成功的Promise的对象
  + 如果抛出异常，结果就是失败的Promise对象

  ```javascript
      async function main() {
        // return 111;
        return new Promise((resolve, reject) => {
          // resolve("成功");
          reject("失败")
        })
        // throw "err"
      }
      let result = main();
      console.log(result)
  ```

### await表达式

+ await必须写在async函数中，但async函数中可以没有await

+ await右侧的表达式一般为promise对象，如果是其他值，将此值作为await的返回值

+ await返回的是promise成功的值

+ 如果await的promise失败了，就会抛出异常，需要通过try...catch捕获处理     

  ```javascript
      async function main() {
        let p1 = new Promise((resolve, reject) => {
          // resolve("成功");
          reject("失败")
        })
        try {
          let res = await p1;
        } catch (err) {
          console.log(err);
        }
      }
  
      main();
  ```

### async和await读取文件

```javascript
const fs = require("fs");

function readWeiXue() {
  return new Promise((resolve, reject) => {
    fs.readFile("./为学.md", (err, data) => {
      if (err) {
        reject(err);
      }
      resolve(data);
    })
  })
}

function readChaYangShi() {
  return new Promise((resolve, reject) => {
    fs.readFile("./插秧诗.md", (err, data) => {
      if (err) {
        reject(err);
      }
      resolve(data);
    })
  })
}

function readGuanShu() {
  return new Promise((resolve, reject) => {
    fs.readFile("./观书有感.md", (err, data) => {
      if (err) {
        reject(err);
      }
      resolve(data);
    })
  })
}

async function main() {
  let res1 = await readWeiXue();
  let res2 = await readChaYangShi();
  let res3 = await readGuanShu();
  console.log(res1.toString());
  console.log(res2.toString());
  console.log(res3.toString());
}

main();
```

### async封装ajax请求

```javascript
 function sendAJAX(url) {
      return new Promise((resolve, reject) => {
        let x = new XMLHttpRequest();
        x.open("get", url);
        x.send();
        x.onreadystatechange = function () {
          if (x.readyState === 4) {
            if (x.status >= 200 && x.status < 300) {
              resolve(x.response);
            }
            reject(x.status)
          }
        }
      })
    }
    // promise then 方法测试
    // sendAJAX("https://api.apiopen.top/getJoke").then(value => {
    //   console.log(value);
    // },reason => {

    // })

    async function main() {
      let res1 = await sendAJAX("https://api.apiopen.top/getJoke");
      console.log(res1);
    }
    main();
```

### 类和对象

* 在ES6中类没有变量提升，所以必须先定义类，才能通过类实例化对象
* 类里面的共有属性和方法一定要加this使用
* constructor里面的this指向实例化对象，方法里面的this指向这个方法的调用者

### new在执行时会做四件事情

* 在内存中创建一个新的空对象
* 让this指向这个空对象
* 执行构造函数里面的代码，给这个新对象添加属性和方法(proto)
* 返回这个新对象(所以构造函数里面不需要return)

### 构造函数

* 构造函数中的属性和方法称为成员，成员可以添加
* 实例成员就是构造函数内部通过this添加的成员，实例成员只能通过实例化的对象来访问
* 在构造函数本身上添加的成员是静态成员，只能通过构造函数访问

```javascript
Star.sex = "男";
console.log(Star.sex);
console.log(ldh.sex) //不能通过对象来访问
```

### 原型

#### 构造函数原型 prototype

每一个构造函数都有一个prototype原型对象，指向另一个对象，默认是空对象，不变的方法直接定义到prototype对象上，所有对象的实例就可以共享这些方法

#### 对象原型 __proto_ _

对象都会有一个属性 _ proto_ 指向构造函数的prototype原型对象，对象可以使用构造函数prototype原型对象的属性和方法，就是因为对象有 _ proto _原型的存在

* _ proto _ 对象原型和原型对象prototype是等价的
* _ proto _对象原型的意义就在于为对象的查找机制提供一个方向，或者说是一条路线，但它是一个非标准属性，实际开发中不可以使用这个属性，它只是内部指向原型对象prototype

#### construc构造函数

​	对象原型和构造函数原型对象里面都有一个属性constructor属性，constructor称为构造函数，因为它指回构造函数本身

​	主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数

​	如果修改了原来的原型对象，给原型对象赋值的是一个对象，则必须手动的利用constructor指回原来的构造函数

```javascript
Star.prototype = {
	constructor:Star
}
```

### 原型链

* 只要是对象就有_ proto _ 原型，指向原型对象
* Star原型对象里面的_ proto _ 原型指向的是Object.prototype
* Object.prototype原型对象里面的_ proto _ 原型指向为null

#### JavaScript的成员查找机制

* 当访问一个对象的属性(包括方法时)，首先查找这个对象本身有没有该属性
* 如果没有就查找它的原型(也就是_ proto _指向的prototype原型对象)
* 如果还没有就查找原型对象的原型(Object的原型对象)
* 以此类推一直找到Object为止(null)

#### 原型对象this指向

* (this调用时才能确定)

* 在构造函数中，里面this指向的是对象实例
  
* 原型对象函数里面的this指向的是实例对象
  =======

* 原型对象函数里面的this指向的是实例对象

### 继承

​	ES6之前没有提供extends继承，可以通过构造函数+原型对象模拟实现继承，称为组合继承

 * call(): 调用这个函数，并且修改函数运行时的this指向

   ```javascript
   fun.call(thisArg,arg1,arg2,...)
   ```

   thisArg: 当前调用函数this的指向对象

   arg1,arg2: 传递的其他参数

#### 借用构造函数继承父类型属性

``` javascript
    function Father(uname, age) {
      this.uname = uname;
      this.age = age;
    }
    Father.prototype.money = function () {
      console.log(10000);
    }

    function Son(uname, age) {
      // 将父类的this修改为子类的this
      Father.call(this, uname, age);
    }
    // 通过赋值原型对象会将父类和子类的原型对象指向同一个地址，子类上添加特有方法后，父类也会添加
    // Son.prototype = Father.prototype;
    // 创建一个Father的实例，此实例可以通过__proto__访问Father原型对象的方法
    Son.prototype = new Father();
    // Son中的constructor指向了Father，
    // 如果利用对象的形式修改了原型对象，需要使用constructor指向原来的构造函数
    Son.prototype.constructor = Son;
    Son.prototype.exam = function () {
      console.log("考试")
    }
    var son = new Son("Tom", 21)
    console.log(son);
    console.log(Father.prototype)
    console.log(Son.prototype.constructor)
```

### ES5新增方法

#### 数组方法

* forEach()：array.forEach(function(currentValue,index,arr))

  currentValue:数组当前项的值

  index:数组当前项的索引

  arr:数组对象本身

```javascript
    let arr = [1, 2, 3, 4, 5];
    let sum = 0;
    arr.forEach((value, index, arr) => {
      sum += value;
    })
    console.log(sum);
```

* filter()：array.filter(function(currentValue,idnex,arr))

  直接返回一个数组

  currentValue:数组当前项的值

  index:数组当前项的索引

  arr:数组对象本身

```javascript
    let arr = [1, 2, 3, 4, 5, 6];
    let res = arr.filter((value) => {
      return value > 2
    })
    console.log(res);
```

* some()：array.some(function(currentValue,index,arr))

  查找数组中是否有满足条件的元素

  返回值是布尔值，查找到返回true，查找不到返回false

#### 字符串方法

* **trim()**：str.trim()

  trim()方法并不影响原字符串本身，它返回的是一个新的字符串

#### 对象方法

* **Object.keys()**用于获取对象自身所有的属性

  Object.keys(obj)

  效果类似for...in

  返回一个由属性名组成的数组

* **Object.defineProperty()**：定义新属性或修改原有的属性

  Object.defineProperty(obj,prop,descriptor)

  * obj：必需。目标对象
  * prop：必需。需定义或修改的属性的名字
  * descriptor：必需。目标属性所拥有的特性

  **descriptor说明**：以对象形式{ }书写

  + value：设置属性的值 默认为undefined
  + writable：值是否可以重写。true|false 默认为false
  + enumerable：目标属性是否可以被枚举。true|false 默认为false
  + configurable：目标和属性是否可以被删除或是否可以再次修改特性。true|false 默认为false

  设置configurable为false后不可再修改

### 函数定义方式

+ 自定义函数(命名函数)

```javascript
function fn(){};
```

+ 函数表达式(匿名函数)

```javascript
var fun = function(){};
```

+ 利用 new Function('参数1'，'参数2'，'函数体');

```javascript
var f = new Function('a','b','console.log(a + b)');
f();
```

​	**所有的函数都是Function的实例，函数也属于对象.**

​	**原型对象中存在constructor属性指回构造函数**

### 函数调用方式和this指向

+ 普通函数									window
+ 对象的方法                               实例对象 原型对象里面的方法也指向实例对象
+ 构造函数                                  该方法所属对象
+ 绑定事件函数                          绑定事件对象
+ 定时器函数                               window
+ 立即执行函数                           window

### 改变this指向

#### apply方法

```javascript
fun.apply(thisArg,[argsArray])
```

+ **thisArg**：在fun函数运行时指定的this值
+ **argsArray**：传递的值，必须包含在数组里面
+ 返回值就是函数的返回值，因为它就是调用函数

+ 可以利用apply借助数学内置对象求数组最大，最小值

  ```javascript
      let arr = [13, 21, 23, 51, 56];
      let max = Math.max.apply(Math, arr);
      console.log(max)
  ```

#### bind方法

​	不会调用函数，但能改变函数内部this指向

```javascript
fun.bind(thisArg,arg1,arg2,...)
```

+ thisArg：在fun函数运行时指定的this值
+ arg1，arg2：传递的其他参数
+ 返回由指定的this值和初始化参数改造的原函数拷贝

```javascript
    let o = {
      name: "Tom"
    }

    function fn() {
      console.log(this);
    }

    let f = fn.bind(o);
    f();
```

应用：有一个按钮，当点击了之后，就禁用这个按钮，3秒钟之后开启这个按钮

```javascript
btn.onclick = function(){
    this.disabled = true; //this指向的是btn这个按钮
    var that = this;
    setTimeout(function(){
		//that.disabled = false;// 定时器函数里面的this指向的是window
        this.disabled = false;// 定时器函数里面的this指向的是window
    }.bind(this),3000)//这个this指向的是btn这个对象
}
```

### call apply bind小结

+ 相同点：都可以改变函数内部的this指向
+ 区别点：
  + call和apply会调用函数，并且改变函数内部this指向
  + call和apply传递的参数不一样，call传递参数aru1，aru2...形式，apply必须数组形式[arg]
  + bind不会调用函数，可以改变函数内部this指向

+ 场景

  call经常做继承

  apply和数组有关系，比如借助数学对象实现数组最大值最小值

  bind不调用函数，但是还想改变this指向，比如改变定时器内部的this指向

### 严格模式

+ 消除了JS语法的一些不合理、不严谨之处，减少了一些怪异行为
+ 消除代码运行的一些不安全之处，保证代码运行的安全
+ 提高编译器效率，增加运行速度

+ 禁用了在ECMAScript的未来版本中可能会定义的一些语法，为未来新版本的Javascript做好铺垫，比如一些保留字如：class，enum，export，import，super不能做变量名

#### 变量规定

+ 变量必须先声明再使用
+ 严禁删除已经声明的变量，例如，delete x；语法是错误的

#### this指向

+ 以前在全局作用域函数中的this指向window对象，严格模式下全局作用域中函数中的this是underfined
+ 以前构造函数时不加new也可以调用，当普通函数，this指向全局对象，严格模式下，如果构造函数不加new调用，this会报错
+ new实例化的构造函数指向创建的对象实例
+ 定时器this还是指向window
+ 事件、对象还是指向调用者

#### 函数变化

+ 函数不能有重名的参数
+ 函数必须声明在顶层，不允许在非函数的代码块内声明函数

### 闭包(Closure)

​	闭包指有权访问另一个函数作用域中局部变量的函数

```javascript
function fn(){
    var num = 10;
    function fun(){
        console.log(num);
    }
    fun();
}
fn();
```

#### 闭包的作用

+ 延伸了变量的作用范围

```javascript
function fn(){
    var num = 10;
    return function fun(){
		console.log(num)
    }
   //return fun;
}
var f = fn();
f();
```

#### 闭包案例

+ 循环注册点击事件

```javascript
    for (var i = 0; i < lis.length; i++) {
      (function (i) {
        lis[i].onclick = function () {
          console.log(i)
        }
      })(i)
    }
```

+ 循环中的setTimeout()

```javascript
var lis = document.querySelector('.nav').querySelectorAll('li');
for(var i = 0;i < lis.length;i++){
	(function(){
		setTimeout(function(){
            console.log(lis[i].innerHTML)
        },3000)
    })(i)
}
```

+ 闭包应用-计算打车价格

  打车起步价13(3公里内)，之后每多一公里增加5块钱，用户输入公里数就可以计算打车价格，如果有拥堵情况，总价格多收取10块钱拥堵费

```javascript
	  var car = (function () {
      var start = 13;
      var total = 0;
      return {
        Car: function (n) {
          if (n <= 3) {
            total = start;
          } else {
            total = (n - 3) * 5
          }
        },
        yd: function (flag) {
          return flag ? total + 10 : total;
        }
      }
    })()
```

#### 思考题

```javascript
    var name = "The window";
    var object = {
      name: "My Object",
      getNameFunc: function () {
        return function () {
          return this.name;
        };
      }
    }
    console.log(object.getNameFunc()()) //输出The window     匿名函数中this指向window，无闭包产生，没有局部变量
```

```javascript
    var name = "The Window";
    var object = {
      name: "My Object",
      getNameFunc: function () {
        var that = this;
        return function () {
          return that.name;
        };
      }
    };
    console.log(object.getNameFunc()()) //输出My Object  that指向object 有闭包，使用了getNameFunc函数的局部变量
```

#### 递归

+ 函数内部自己调用自己

```javascript
    let num = 1;
    function fn() {
      console.log("打印");
      if (num === 6) {
        return;
      }
      num++;
      fn();
    }
    fn();
```

+ 递归求阶乘

```javascript
    function fn(n) {
      if (n === 1) {
        return 1;
      }
      return n * fn(n - 1);
    }
    console.log(fn(3));
```

+ 递归求斐波那契数列

```javascript
    function fn(n) {
      if (n === 1 || n === 2) {
        return 1;
      }
      return fn(n - 1) + fn(n - 2);
    }
    console.log(fn(6))
```

#### 深拷贝和浅拷贝

+ 浅拷贝只是拷贝一层，更深层次对象级别的只拷贝引用
+ 深拷贝拷贝多层，每一级别的数据都会拷贝

+ Object.assign(target,...sources) es6新增方法可以浅拷贝
+ 浅拷贝：

```javascript
    let obj = {
      id: 1,
      name: "andy",
      msg: {
        age: 18
      }
    }
    let o = {};
    for (let k in obj) {
      //k是属性名   obj[k]是属性值
      o[k] = obj[k];
    }
    console.log(o)
    o.msg.age = 20; //o和obj的msg指向同一个地址，修改o里面msg会改变obj
    console.log(obj)

    // es6的assign方法
    Object.assign(o, obj);
```

+ 深拷贝：

```javascript
    let obj = {
      id: 1,
      name: "andy",
      msg: {
        age: 18
      },
      color:["pink","red"]
    }
    let o = {};

    function deepCopy(newobj, oldobj) {
      for (let k in oldobj) {
        let item = oldobj[k];
        if (item instanceof Array) {
          // 判断值是否属于数组
          newobj[k] = [];
          deepCopy(newobj[k], item)
        } else if (item instanceof Object) {
          // 判断值是否属于对象
          newobj[k] = {};
          deepCopy(newobj[k], item)
        } else {
          newobj[k] = item
        }
      }
    }
    deepCopy(o, obj);
    console.log(o)
```

#### 正则表达式

```javascript
    // 1.利用RegExp对象来创建正则表达式
    var regexp = new RegExp(/123/);
    console.log(regexp);

    // 2.利用字面量创建正则表达式
    var rg = /123/;
    // 3.test方法用来检测字符串是否符合正则表达式要求的规范
    console.log(rg.test(123));
    console.log(rg.test("abc"));
```

+ *号出现0次或更多次
+ +号出现1次或更多次
+ ？号出现0次或1次
+ {3}重复3次
+ {3，}大于等于3
+ {3,16}大于等于3并且小于等于16