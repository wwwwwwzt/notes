# JS / ES6 学习笔记

##### js 三大部分：

- ECMA Script 语法标准
- DOM 通过文档模型 dom 操作网页
- BOM 通过浏览器模型 bom 操作浏览器

##### 解释/编译：

- 解释：翻译一行执行一行，不产生编译文件。
  - 解释的时候需要解释环境，运行慢。
  - 有解释环境就行，可以执行好。
  - js python
- 编译：生成编译文件在执行。
  - 速度快，代码效率高，保密性好。
  - 可以执行差
  - c++

##### js 特点：

- 单线程：与用户互动、操作 DOM
- .css .js 分离 使用外部引入
- 变量提升：将变量的声明提到前面。

##### 栈内存 / 堆内存：

&emsp;Js 分为栈内存和堆内存。栈内存用来存放基础类型的数据（也叫原始值） number boolean string null symbol undefined，堆内存存放复杂类型（也叫引用值）：对象。
<img src="2023-10-10-17-16-36.png" style="zoom:70%;"/>

- 复制值的时候，原始值的两个变量互不干扰`let num1 = 5,num2 = num1`
- 复制值的时候，引用值复制的实际上是一个指针，指向存储在堆内存中的对象。操作完成后，两个变量实际上指向同一个对象。

##### JS 的事件循环

- js 是一门单线程的非阻塞的脚本语言。
- **单线程**：如果有两个线程同时对同一个 DOM 进行操作，一个是在这个 DOM 上绑定事件，另外一个是删除该 DOM，此时就会产生歧义。因此 JS 以单线程来执行代码，保证一致性。
- **非阻塞**：当 JS 代码从上往下执行，遇到需要进行一项异步任务的时候，主线程会挂起这个任务，继续往下执行代码，然后在异步任务返回结果的时候再根据一定规则去执行。
  <img src="images/2024-03-18-16-27-27.png" style="zoom:20%;"/>
- 异步任务分类：宏任务（setTimeout），微任务（promise）
- 所有同步任务都在主线程上执行，形成一个执行栈
- 遇到异步任务放到任务表中，等事件执行完成（ajax 请求完成、setTimeout 设置时间到期），之后放入到任务队列
- 当执行栈的同步任务执行完成之后，就会执行任务队列的第一个异步任务，其中把宏观任务和微观任务都执行完成后才进行下一次循环。

###### 函数传递参数

- 所有函数都是按值传递的。函数外部的值会被复制到函数内部的参数中。这样，函数外部的参数和函数内部的参数都指向同一个对象。所以，在函数内修改对象中的内容，会影响函数外部。
- 但是当函数内部的参数被重新定义为一个新的对象时`obj = new Object()`，他实际上就指向了一个新的 object。而函数外的变量依旧指向原来的 object，不会被影响。
- 这说明对象在函数中也是按值传递的，而不是按引用传递。

##### 执行上下文栈和作用域链

- 执行上下文就是当前 JavaScript 代码被解析和执行时所在环境, JS 执行上下文栈可以认为是一个存储函数调用的栈结构。
- 当一段 JavaScript 代码在运行的时候，它实际上是运行在执行上下文中。

  - JavaScript 执行在单线程上，所有的代码都是排队执行。
  - 浏览器执行全局的代码时，首先创建全局的执行上下文，压入执行栈的顶部。
  - 每个函数会在执行的时候创建自己的执行上下文，并且把它压入执行栈的顶部。这个上下文就是
  - 通常说的 “本地上下文”。当前函数执行完毕并退出的时候，其上下文也从栈中弹出并销毁。
  - 主程序退出，全局执行上下文从执行栈中弹出。此时栈中所有的上下文都已经弹出，程序执行完毕。

- 作用域链
  函数执行所需要的变量在当前作用域中找不到的时候，它就会一层一层向上查找，一直找到全局作用域。这种一层一层的关系，就是作用域链。

##### new

- 创建了新空对象
- 新对象的`\_\_proto\_\_ = 构造函数.prototype`
- 将构造函数的作用域赋值给新对象(this 指向新对象)
- 执行构造函数代码 （为这个新对象添加属性）
- 返回新对象

```js
function Student(obj) {
  // this={};
  this.name = obj.name;
  this.score = obj.score;
  this.grade = obj.grade;
  // return this;
}
var a = new Student({ name: '1', score: '2', grade: '3' });
```

##### var let const

- var：函数作用域。 有变量提升。
- let：块级作用域。没有变量提升，即暂时性死区（声明前不可用）。
- const：同 let。变量声明的同时立即赋值。如果是简单类型则不可改变。

```js
if (true) {
  var a = 0;
  const b = 1;
  let c = 2;
}
//a为0; b/c is not defined
console.log(a, b, c);
```

##### 数据类型

- 原始类型数据：Number String Boolean Null Undefined Symbol
- 复杂类型数据：Object

- Undefined：没初始化的变量就是 undefined。undefended 唯一有用的事情就是 typeof。有意思的是，没声明和没初始化的变量都`typeof == undefined`。
- null：表示一个空指针对象，`typeof null == object`。有趣的是，`null == undefined`。
- number：
  - 浮点数
    ```js
    //由于浮点数的精度高达17位，在计算中很不精准。
    //例如a=0.1，b=0.2，c却可能是0.30000000000004
    if (a + b == 0.3) {
      console.log('0.3');
    }
    ```
  - NaN `NaN == NAN 结果为false`变量
    `isNaN(NaN)`或者`isNaN(转换不成数字的)`返回值为 true
  - Infinity `5/0会返回Infinity 5/-0会返回-Infinity`
  - `Number(null) == 0` `Number(undefined) == NaN`
  - parseInt() parseFloat()较 Number()更为常用。如果第一个非空格字符不是数字、加减号，则直接返回 NaN。因此`parseInt(null) == NaN`，与 Number()不同。
- String
  - 模板字面量 + 字符串插值 \`${var1}\`
- Symbol：对象的属性名容易产生命名冲突，为保证键名的唯一性，故 es6 引入 Symbol 这种新的原始类型数据，确保创建的每个变量都是独一无二的。
  ```js
  let school1 = Symbol('bjut');
  let school2 = Symbol('bjut');
  console.log(school1 === school2); //false
  ```
  - 由于 Symbol 函数返回的值是原始类型的数据，不是对象，故 Symbol 函数前不能使用 new 命令，否则会报错。
  - 也可以使用 Symbol.for()来全局的定义到全局符号注册表中。
    ```js
    let school1 = Symbol.for('bjut');
    let school2 = Symbol.for('bjut');
    console.log(school1 === school2); //true
    ```

##### [] == ![] 类型转换

<img src="images/2024-03-20-23-15-40.png" style="zoom:30%"/>

为什么[] == ![]的结果为 true：
  - 因为

##### 原型链

JavaScript 是动态的，本身不提供一个 class 的实现。即便是在 ES6 中引入了 class 关键字，但那也只是语法糖，JavaScript 仍然是基于原型的。

- 当谈到继承时，JavaScript 只有一种结构：对象。每个实例对象（object）都有一个私有属性（称之为 \_\_proto\_\_）指向它的构造函数的原型对象（prototype）。该原型对象也有一个自己的原型对象（\_\_proto\_\_），层层向上直到一个对象的原型对象为 null。根据定义，null 没有原型，并作为这个原型链中的最后一个环节。
- 创建一个函数，就会给函数创建一个 prototype 属性（指向原型对象）。

- 原型
  在 JavaScript 中，每个函数对象都有一个 prototype 属性，这个属性指向函数的原型对象。使用原型对象的好处是所有对象实例共享它所包含的属性和方法。
- 原型链
  每个对象拥有一个原型对象，通过 \_\_proto\_\_ (double underscore proto) 指向其原型对象，并从中继承方法和属性。同时原型对象也可能拥有原型，这样一层一层，最终指向 null(Object.prototype.\_\_proto\_\_ 指向的是 null)。这种关系被称为原型链。通过原型链，一个对象可以拥有定义在其他对象中的属性和方法。
  \_\_proto\_\_ 翻译做隐式原型链。因为一般双下划线开头并结尾的属性和方法是不希望被直接使用和操作的，这种属性通常称它为隐式。
- prototype 和 \_\_proto\_\_

  - prototype 是构造函数的属性。
  - \_\_proto\_\_ 是每个实例都有的属性，指向其原型对象。
  - 实例的 \_\_proto\_\_ 与其构造函数的 prototype 指向的是同一个对象。

  ```js
  function Person() {}
  var person = new Person();
  console.log(person.__proto__ === Person.prototype); //true
  ```

- constructor
  每个原型都有一个 constructor 属性指向关联的构造函数。

  ```js
  function Person() {}
  console.log(Person === Person.prototype.constructor); //true
  ```

- 原型对象也可能拥有原型，这样一层一层，最终指向 null。
  <img src="2024-03-11-17-28-45.png" style="zoom:40%"/>

##### 判断数组的方式

```js
arr instanceof Array;
Array.isArray(arr);
Array.prototype.isPrototypeOf(arr);
arr.constructor === Array;
Object.prototype.toString.call(arr).slice(8, -1) === 'Array';
//这玩意会返回一个数组[object Array] 神奇
```

##### 深拷贝

`JSON.parse(JSON.stringify(obj))`

- 基本类型数据不具备深浅拷贝。赋值既不是深拷贝也不是浅拷贝，只是跟深拷贝是类似。
- concat()、slice()对一维数组来说是深拷贝，多维数组的话是浅拷贝

##### for-in for-of

for-in 用于枚举 object 中的非 Symbol 的“key” `for(const key in obj)`
for-of 用于遍历可迭代的对象的元素。如 Array，String，Map，Set 等。`for (let value of arr)`

##### 解构复制

&emsp;&emsp;可以理解为赋值操作的语法糖。对数组或对象进行模式匹配，然后对其中的变量进行赋值。

- 数组解构
  ```javascript
  let [a, b, c = 6] = [1, 2]; //变量赋值及默认值
  let [a, ...other] = [1, 2, 3]; //分割数组 other[2,3]
  ```
- 对象解构
  ```javascript
  //var1 = '123' var2 = 123
  let { property1: var1, property2: var2 } = { property1: '123', property2: 123 };
  ```
- 读取接口返回的数据
  ```javascript
  function xd() {
    return {
      name: 'xdclass',
      wangzhi: [
        {
          url: 'xdclass.net',
        },
      ],
    };
  }
  let {
    name,
    wangzhi: [{ url }],
  } = xd();
  ```

##### 扩展运算符 `...`

- 扩展数组
  - 深拷贝一维数组（不能深拷贝二维数组）
    ```javascript
    const list = [1, 2, 3, 4, 5];
    const list_new = [...list];
    ```
  - 分割数组
    ```javascript
    const list = [1, 2, 3, 4, 5];
    const [a, , ...list_new] = list;
    //list_new [3,4,5]
    ```
  - 将数组转化成参数传递给函数
    ```javascript
      const list = [1,2];
      function xd(a,b) {
        ...
      }
      xd(...list);
    ```
- 扩展对象
  - 深拷贝简单对象 `const b = {..a}`
  - 与解构赋值配合
    ```js
    let object = { a: '1', b: '2', c: '3', d: '4' };
    let { a, b, ...other } = object; //object {c: '3', d: '4'}
    ```
  - 合并对象
    ```js
    const a = {aa:1, bb:2};
    const b = {cc:3, dd:4};
    const c = {..a, ..b}; //c {aa:1, bb:2, cc:3, dd:4}
    ```
- find findIndex 函数（返回子项）
  ```js
  const list = [
    { name: 'zs', age: 10 },
    { name: 'ls', age: 11 },
    { name: 'ww', age: 12 },
    { name: 'zl', age: 10 },
  ];
  //此处返回的不是boolean值，而是找到的第一个满足条件的对象
  const result = list.find((item) => {
    return item.age === 10;
  });
  //也可以简写为
  const result2 = list.find((item) => item.age === 10);
  ```
- filter 函数（返回数组）
  ```js
  //此处返回的不是boolean，而是一个新的对象数组
  const result = list.filter((item) => {
    return item.age === 10;
  });
  ```

##### map 方法

- map
  ```js
  //返回一个新的对象数组，其中的每个对象就是return{}中的内容
  const list_new = list.map(function (item) {
    return {
      id: item.name,
      age_state: item.age <= 18 ? '未成年' : '成年人',
    };
  });
  ```
- 对象的深拷贝
  ```js
  const i = { a: 123, b: '123' };
  let obj = {};
  Object.assign(obj, i);
  ```

##### 属性、方法的简写

```js
ES5           ------>         ES6
let obj = {                   let obj={
  name: name,                   name,
  age: age,                     obj,
  sayHello(): function(){       sayHello(){
    ...                           ...
  }                             }
}                             }
```

##### Object 方法

```js
Object.is(a, b) 代替===符号的写法。唯一的不同是，NaN===NaN会判断为false，而Object.is(NaN, NaN)会返回true。
Object.assign(target,  source) 把source对象传入target对象.
Object.keys(object) 返回一个包含所有属性的数组
Object.values(object) 返回一个包含所有属性值的数组
Object.entries(object) 把对象变为数组 //[['name','wzt'],['age',18],['address',1513]]
```

##### Map WeakMap 对象

&emsp;&emsp;**js 中的对象，实质就是键值对的集合**。但是在对象里只能使用字符串作为键名。因此，Map 对象被提出了，它是 js 中一种更完善的 Hash 结构。

```js
const map = new Map([
  ['name1', 'zhangsan'],
  [3.14159, 'pai'],
  [undefined, 123],
])
map.size() //键值对的数量
map.clear() //清空Map
map.has(key)  //是否有指定的键名，返回Boolean
map.get(key)  //获取指定键名的键值，没有则返回undefined
map.set(key, value) //添加键值对
map.delete(key) //删除指定键名的键值对
map.keys() map.values() map.entries() //同Object
```

&emsp;&emsp;**WeakMap 对象**。键名只能是对象，不能是字符串、数值。

```js
const weakMap = new WeakMap([document.getElementById('test'), element]);
```

##### Set WeakSet 结构

&emsp;&emsp;Set 是一种类似数组的数据结构，可以理解为值的集合。它的值不会有重复项。

```js
const list = new Set([1, 2, 3, 4]);
list.keys() === list.values(); //key value相等

let arr = [1, 2, 3, 3, 3, 3, 4, 5];
let set = new Set(arr); //数组的去重
```

&emsp;&emsp;WeakMap 结构不可遍历。因为它的成员都是对象的弱引用，随时被回收机制回收，成员消失。

##### 类型转换

```js
//对象转Map
const obj = { school: 'BJUT', class: '814' };
const map = new Map(obj.entries());
//Map转对象
const obj = Object.fromEntries(map);
//数组转Set
const list = [1, 2, 3];
const set = new Set(list);
//Set转数组
const list = Array.from(set);
```

##### Proxy Reflect（没完全理解）

&emsp;&emsp;Proxy 不直接作用在对象上，而是作为一种媒介。如果需要操作对象的话，需要经过这个媒介的同意。

```js
const house = {
  name: '张三',
  price: '1000',
  phone: '18823139921',
  id: '111',
  state: '**',
};

const houseProxy = new Proxy(house, {
  // 读取代理
  get: function (target, key) {
    switch (key) {
      case 'price':
        return target[key].replace('1000', '1200');
      case 'phone':
        return target[key].substring(0, 3) + '****' + target[key].substring(7);
      default:
        return target[key];
    }
  },
  // 设置代理
  set: function (target, key, value) {
    if (key === 'id') {
      return target[key];
    } else if (key === 'state') {
      return (target[key] = value);
    }
  },
});
```

Reflect 没有仔细讲，我暂时没理解。

##### 使用 Proxy 实现简单的双向数据绑定

```js
const input = document.getElementById('input');
const span = document.getElementById('span');

const obj = {};
const handler = {
  get: function (target, key) {
    return target[key];
  },
  set: function (target, key, value) {
    if (key === 'text' && value) {
      //将js对象中的改变绑定到html组件中
      span.innerHTML = value;
      return (target[key] = value);
    }
  },
};
const inputProxy = new Proxy(obj, handler);

input.addEventListener('keyup', function (e) {
  //"keyup"为获取键盘输入
  //将html组件中的改变绑定到js对象中
  inputProxy.text = e.target.value; //获取键盘输入
  console.log(inputProxy.text);
});
```

##### rest 参数

形式为"...变量名"，用于获取函数的多余参数。

```js
function xd(a, ...rest) {
  //a 为 1, rest 为 [2，3]
}
xd(1, 2, 3);
```

##### 箭头函数

- 更短的函数 `() => {}`
- 不绑定 this
- 没有**_arguments_** （获取函数入参）
  以下情况 **_不能_** 使用箭头函数：
- 定义对象
  ```js
  const object = {
    room: 814,
    sum: () => {
      this.room; //此处无法得到object的room。因为this指向的是Window
    },
  };
  ```
- 定义 DOM 事件的回调函数
  ```js
  const h3 = document.getElementById('h3');
  h3.addEventListener('click', () => {
    this.innerHTML = 11;
  });
  ```
- 定义构造函数
  ```js
  const Car = () => {
    console.log(this);
  };
  ```

##### 类

```js
class Parent {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
//继承
class Student extends Parent {
  constructor(name, age, grade) {
    super(name, age); //super是在子类的构造函数中调用父类的构造函数
    this.grade = grade;
  }
}
```

##### 模块化开发 import export

&emsp;&emsp;使用 import 的时候会报错`Cannot use import statement outside a module`，此时需要在 html 中加入`<script src="./chap5.js" type="module" />`

```js
export const a = 1;       import * as test from '...' //test对象获得了所有导入
export const b = 2;       import {a, b} from '...'  //分别导入

export default {          import test from '...'
  //全部导出                //test对象获得了所有导入
};
```

##### Promise

- Promise 是一个容器，里面保存着某个未来才会结束的事件（通常为异步）的结果
- Promise 是一个对象，它可以获取异步操作的最终状态（成功或失败）
- Promise 是一个构造函数，提供统一的 API。里面也可以放同步的代码。

回调地狱 （可读性差、代码耦合、出错时无法排除）

```js
function request() {
  axios.get('a.json').then((res) => {
    if (res && res.data.code === 0) {
      axios.get('b.json').then((res) => {
        if (res && res.data.code === 0) {
          axios.get('c.json').then((res) => {});
        }
      });
    }
  });
}
```

Promise

```js
new Promise((resolve, reject) => {  //初始状态pending
  resolve('成功');  //成功状态resolve
  reject('失败');   //失败状态rejected
})
// .then(,)有两个入参，第一个成功时执行，第二个失败时执行。
// 第二个入参的效果等同于.catch()，用.catch居多。
.then((res) => {
  console.log(res);
},(err)=>{
  console.log(err);
});
.catch((err) => {
  console.log(err);
});
```

Promise 解决回调地狱

```js
function request() {
  return new Promise((resolve, reject) => {
    axios.get('a.json').then((res) => {
      if (res && res.status === 200) {
        resolve(res.data.data.data);
      } else {
        reject('a接口请求失败');
      }
    });
  });
}

function request() {
  requestA()
    .then((res) => {
      console.log(res);
      return requestB();
    })
    .then((res) => {
      console.log(res);
      return requestC();
    })
    .then((res) => {
      console.log(res);
    })
    .catch((err) => {
      console.log(err);
    });
}
```

Promise.all()方法
&emsp;&emsp;此方法接受一个 promise 数组并返回一个 promise。然后当所有的 promise 都完成时会得到结果数组。当其中一个被拒绝时会抛出错误。

```js
function requestAll() {
  Promise.all([requestA(), requestB(), requestC()])
    .then((res) => {
      console.log(res);
    })
    .catch((err) => {
      console.log(err);
    });
}
```

Promise.race()方法
&emsp;&emsp;Promse.race() 就是赛跑的意思，传入的 promise 数组里面哪个结果获得的快，就返回那个结果。不管结果本身是成功状态还是失败状态。

##### async

&emsp;&emsp;async 是异步的简写，Generator（不重要）的语法糖，用于声明一个函数是异步函数。

async await 改造 promise.then 异步调用

```js
function requestA() {
  return new Promise((resolve, reject) => {
    axios.get('a.json').then((res) => {
      setTimeout(() => {
        resolve(res);
        console.log('aaa');
      }, 1000);
    });
  });
}
async function request() {
  try {
    const a = await requestA();
    const b = await requestB();
    const c = await requestC();
    console.log(a, b, c);
  } catch (err) {
    console.log(err);
  }
}
```

##### 闭包

闭包是指有权访问另一个函数作用域中的变量的函数，创建闭包最常用的方式就是在一个函数内部创建另一个函数。闭包可以读取到另个一函数内部的变量，让这些变量的值始终保存在内存中。

- 全局上下⽂ window
- 函数上下⽂
  函数执⾏的时候会形成⾃⼰的上下⽂(环境，对象)。
  函数执⾏结束的时候函数上下⽂就会销毁。
- 闭包：有权访问另⼀个函数作⽤域中的变量的函数。是通过调⽤函数时返回其内部的函数。
  JavaScript 变量属于本地或全局作⽤域。全局变量能够通过闭包实现局部（私有），也就是只有调⽤函数才能改变变量。
- 闭包的副作⽤：产⽣内存泄漏。⽐如说我本来要销毁函数的上下⽂，被强⾏保存下来了，保存在内存当中。

```js
var Counter = (function () {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function () {
      changeBy(1);
    },
    decrement: function () {
      changeBy(-1);
    },
    value: function () {
      return privateCounter;
    },
  };
})(); //不加最后的括号，Counter就是个立即执行函数。加了括号后，就变成函数的返回值。
console.log(Counter.value()); /* logs 0 */
Counter.increment();
console.log(Counter.value()); /* logs 1 */
Counter.decrement();
console.log(Counter.value()); /* logs 0 */
console.log(Counter); //{increment: ƒ, decrement: ƒ, value: ƒ}
```
