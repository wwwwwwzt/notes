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

&emsp;Js 分为栈内存和堆内存。栈内存用来存放基础类型的数据 number boolean string null symbol undefined，堆内存存放复杂类型：对象、数组。
![](2023-10-10-17-16-36.png)

##### var let const

- var：函数作用域
- let：块级作用域。没有变量提升。暂时性死区（声明前不可用）。
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

##### 解构复制

&emsp;可以理解为赋值操作的语法糖。对数组或对象进行模式匹配，然后对其中的变量进行赋值。

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

##### map
- map
  ```js
  //返回一个新的对象数组，其中的每个对象就是return{}中的内容
  const list_new = list.map(function(item){
    return {
      id: item.name,
      age_state: item.age <= 18 ? "未成年":"成年人",
    }
  })
  ```
- 对象的深拷贝
  ```js
  const i = {a: 123, b: "123"}
  let obj = {};
  Object.assign(obj, i);
  ```
