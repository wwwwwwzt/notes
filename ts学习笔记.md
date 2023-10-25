# ts 学习笔记

##### 字面量 literal

`let foo: 'Hello' = 'Hello';` foo 变量的类型是 ‘Hello’，它只能兼容字面量值为 ‘Hello’ 的变量，也只能接受 ‘Hello’ 作为赋值数据。
与联合类型声明结合：`let season: "spring"|"summer"|"fall"|"winter";`

##### any 类型（尽量不使用）

&emsp;&emsp;表示任意数据类型，一个变量设置类型为 any 后相当于对该变量关闭了 TS 的类型检测。
`let b; //隐私的any类型声明，声明如果不指定类型，则TS解析器会自动判断变量的类型为any`

```ts
let b;
b = 'bjut';
let c: number;
c = b; //any类型变量b关闭了number型c的类型检测
```

##### unknown 类型（安全的 any）

```typescript
let c: unknown;
c = '123';
let d: string;
d = c; //此时TS解析器提示是报错的
if (typeof c === 'string') {
  d = c; //这样就可以完成赋值了
}
```

##### 类型断言

可以用来告诉 TS 解析器变量的实际类型，变量 as 类型。上面 unknown 的代码也可以写成`d = c as 'string'`

##### 函数返回值

- 没有定义返回值类型的时候，返回 undefined。

```ts
function fun(): void {
  //声明返回值类型void时，以下情况均不报错
  return undefined;
  return null;
  return;
}
```

- never 是永不返回函数的返回类型，也是变量在类型保护中永不为 true 的类型。

  ```ts
  function handleValue(val: All) {
    // 此处的All.type由'a'与'b'组成
    switch (val.type) {
      case 'a':
        break;
      case 'b':
        break;
      default: // val 在这里是 never
        const exhaustiveCheck: never = val;
        break;
    }
  }
  ```

&emsp;&emsp;假如后来有一天改了 All.type 为'a''b'和'c'，然而忘记在 handleValue() 里面加上针对 c 的处理逻辑，这个时候在 default branch 里面 val 会被收窄为 c，导致无法赋值给 never，产生一个编译错误。所以通过这个办法，你可以确保 handleValue 总是穷尽 (exhaust) 了所有 All 的可能类型。

##### ts 对象

```ts
let object1: {
  //后续赋值的对象必须包含name；age和sex可有可无；其他属性不能有
  name: string;
  age?: number; //实际上相当于 number|undefined
  sex?: string;
};
object1 = { name: 'name1' };
object1 = { name: 'name2', age: 12 };
```

当我们只知道我们必须有的属性，而其他的不必须我们不知道时：

```ts
//这个对象必须含有name属性，也还可以有其他可选属性，只要属性名满足是字符串，属性值是unknown即可。
//propName:这个任意命名，就表示属性的名字，这个属性名字的类型是string。js中属性名一般都是用string定义
let b: {
  name: string;
  [propName: string]: unknown;
};
```

##### ts 数组

```ts
let myArray: string[]; //此处必须有一个类型，不知道可以写any/undefined
let myArray: Array<number>;
```

##### tuple 元组

数组中的数量是固定的就叫做元组。元组的存储效率比较好点，因为不会出现扩容的现象。`let h:[string, int, string];`

##### enum 枚举

数字枚举。默认情况下，第一个枚举值是 0，后续至依次增 1

```ts
enum Color {
  red, //Color.red 就是 0
  blue, //Color.blue 就是 0
  yellow, //Color.yellow 就是 0
}
enum gender {
  male = -2, //gender.male 就是-2
  female, //gender.male 就是-1
  l, //gender.male 就是0
  b = 0, //也是0
}
```

字符串枚举

```ts
enum gender {
  male = 'male',
  female = 'female',
}
```

##### 类型别名

```ts
type oneToSix = 1 | 2 | 3 | 4 | 5 | 6;
let a: oneToSix;
let b: oneToSix;
```

##### tsconfig.json

在项目的根目录下创建一个 ts 的配置文件 tsconfig.json。添加配置后，只需要 tsc 命令即可完成对整个项目的编译。

```json
{
  //定义希望被编译文件所有的目录
  "include": ["./dev"],
  //不被编译文件所有的目录
  "exclude": ["./config"],
  //定义被继承的配置文件
  "extends": ["./config/base"],
  //指定需要编译文件的列表
  "files": ["one.ts", "two.ts", "three.ts", "four.ts"],

  "compilerOptions": {
    //指定ts编译的js目标版本
    "target": "ES6",
    //指定使用的模块化规范
    "module": "AMD",
    //指定编译过程中需要引入的库文件的列表（不太会使用）
    "lib": ["DOM"],
    //用来指定编译后文件所在的目录
    "outDir": "./dist",
    //将编译的代码合并成一个文件
    "outFile": "./dist/main.js"
  }
}
```

```json
"compilerOptions": {  //严格的代码规范检查
    //设置编译后的文件是否使用严格模式
    "alwaysStrict": true,
    //不允许隐式的any类型
    "noImplicitAny": true,
    //严格的检查空值
    "strictNullChecks": true,
    //所有严格的总开关
    "strict": false
  }
```

##### class 类

```ts
class Person {
  name: string;
  static location: 'BJUT'; //static属于类，不属于类的实例
  readonly age: number; //只读
  constructor(name: string) {
    this.name = name;
  }
}
var zcl = new Person('zcl'); //也可以写做var zcl = new Person;
class Wzt extends Person {
  constructor(name: string) {
    //子类不写构造方法，父类将自动调用。
    super(name); //如果子类写构造方法，则会把父类构造方法覆盖。所以必须调用父类的构造函数，即super()
  }
}
```

##### abstract 抽象类

```ts
abstract class Animal {
  //抽象类没有实例，专门用来被继承。
  name: string = '抽象类';
  abstract sleep(): void; //抽象方法没有方法体
}
class Dog extends Animal {
  bark() {} //子类继承抽象类，必须重写抽象类中的全部抽象方法
}
```

##### ts 接口

```ts
interface myInterface {
  name: string;
  age: number;
  sayHello(): void;
}
//在定义类时，可以使类去实现一个接口
//实现接口就是使类满足接口的要求
class whatever implements myInterface {
  name: string; //必须包含接口中的内容
  age: number;
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(this.name);
  }
}
```

##### 属性封装

属性修饰符

- public：修饰的属性可以在任意位置访问（修改），是默认值
- private: 私有属性，私有属性只能在类内部进行访问（修改）
- protected: 受保护的属性，只能在当前类和当前类的子类中访问（修改）

属性存取器 getter setter

```ts
class Person {
  private name: string;
  constructor(name: string) {
    this.name = name;
  }
  get name() {
    return this.name;
  }
  set name(name: string) {
    this.name = name;
  }
}
console.log(p1.name); // 通过getter读取name属性
p1.name = 'zcl'; // 通过setter修改name属性
```

##### 泛型

```ts
function cache<T, P>(value1: T, value2: P): T {
  return T;
}
cache<string, number>('123', 456);
```

##### The end...

<div style="text-align:right;">2023/10/25 Oscar</div>
