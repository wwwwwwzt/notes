# ts 学习笔记

##### 字面量 literal

`let foo: 'Hello' = 'Hello';` foo 变量的类型是 ‘Hello’，它只能兼容字面量值为 ‘Hello’ 的变量，也只能接受 ‘Hello’ 作为赋值数据。

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

- ```ts
  function fun(): void {
    //声明返回值类型void时，以下情况均不报错
    return undefined;
    return null;
    return;
  }
  ```

- ​never 是永不返回函数的返回类型，也是变量在类型保护中永不为 true 的类型。

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
