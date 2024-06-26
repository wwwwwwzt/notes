# vue3 学习笔记

##### 浏览器解析渲染页面

浏览器解析渲染页面分为五个步骤：

- 根据 HTML 解析出 DOM 树
- 根据 CSS 解析生成 CSS 规则树
- 结合 DOM 树和 CSS 规则树，生成渲染树
- 根据渲染树计算每一个节点的信息
- 根据计算好的信息绘制页面
  ![](images/2024-04-01-16-07-41.png)
- 根据 HTML 解析 DOM 树：
  根据 HTML 的内容，将标签按照结构解析成为 DOM 树，DOM 树解析的过程是一个深度优先遍历。即先构建当前节点及其所有子节点，再构建下一个兄弟节点。在读取 HTML 文档，构建 DOM 树的过程中，若遇到 script 标签，则 DOM 树的构建会暂停，直至脚本执行完毕。
- 根据 CSS 解析生成 CSS 规则树：
  解析 CSS 规则树时 js 执行将暂停，直至 CSS 规则树就绪。浏览器在 CSS 规则树生成之前不会进行渲染。
- 结合 DOM 树和 CSS 规则树，生成渲染树：
  DOM 树和 CSS 规则树全部准备好了以后，浏览器才会开始构建渲染树。精简 CSS 并可以加快 CSS 规则树的构建，从而加快页面相应速度。
- 根据渲染树计算每一个节点的信息（布局）：
  布局：通过渲染树中渲染对象的信息，计算出每一个渲染对象的位置和尺寸。回流：在布局完成后，发现了某个部分发生了变化影响了布局，那就需要倒回去重新渲染。
- 根据计算好的信息绘制页面：
  绘制阶段，系统会遍历呈现树，并调用呈现器的“paint”方法，将呈现器的内容显示在屏幕上。
  重绘：某个元素的背景颜色，文字颜色等，不影响元素周围或内部布局的属性，将只会引起浏览器的重绘。
  回流：某个元素的尺寸发生了变化，则需重新计算渲染树，重新渲染。

##### vue

- 核心功能：

  - 声明式渲染：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系。

  - 响应性：Vue 会自动跟踪 JavaScript 状态并在其发生变化时响应式地更新 DOM。vue 使用虚拟 DOM+Diff 算法，只更新变化的 DOM 节点，复用不变的 DOM 节点。

- 单文件组件
  Vue 的单文件组件会将一个组件的逻辑 (JavaScript)，模板 (HTML) 和样式 (CSS) 封装在同一个文件里。单文件组件 (Single-File Components) 是 Vue 的标志性功能。

- Vue3 优点： 1.更小的打包体积和内存 2.页面第一次的渲染速度和更新速度更快 3.更好的支持 TypeScript 4.新增了组合式 API 和内置组件
- 选项式 API 和组合式 API

  - 使用选项式 API，我们可以用包含多个选项的对象来描述组件的逻辑，例如 data、methods 和 mounted。选项所定义的属性都会暴露在函数内部的 this 上，它会指向当前的组件实例。选项式将响应性相关的细节抽象出来，并强制按照选项来组织代码，从而对初学者而言更为友好。
    ```js
    <script>
    export default {
      // data() 返回的属性将会成为响应式的状态，并且暴露在 this 上
      data() {
        return {...}
      },
      methods: {...},
      mounted() {...}
    }
    </script>
    ```
  - 组合式 API 的核心思想是直接在函数作用域内定义响应式状态变量，并将从多个函数中得到的状态组合起来处理复杂问题。这种形式更加自由。

    ```html
    <script setup>
      import { ref, onMounted } from 'vue';

      const count = ref(0);
      function increment() {
        count.value++;
      }
      onMounted(() => {
        console.log(`The initial count is ${count.value}.`);
      });
    </script>
    ```

- MVVM
  ![](2024-02-28-23-37-55.png)
  所以实例化的 Vue 一般被称作 VM，即 ViewModel。

  ```js
  let vm = new Vue({
    el: #app,
    data: {......}
  })
  ```

- 数据代理（难） （可以去官方文档找找）

  - 原理：使用 Object.defineProperty()通过一个对象代理另一个对象属性的读写

  ```js
  Object.defineProperty('目标对象', '代理属性', {
    get() {             //当读取’目标对象‘的’代理属性‘时，getter就会被调用，且返回代理属性的值
      return ......;
    },
    set(value) {        //当修改’目标对象‘的’代理属性‘时，setter就会被调用，且收到修改的值
      ......;
    },
  });
  ```

  - 实现
    <img src="2024-02-29-00-05-06.png" style="zoom:30%;"/>
    通过 Object.defineProperty()把 data 中的属性添加到 vm 对象上，每个属性都有 setter/getter。通过 vm 对象代理 data/\_data 中属性的读写。
    比如`{{msg}}`而不是`{{vm.msg}}`，更加方便的读写 vue 中 data 的数据。

- 被 vue 管理的函数都应写成普通函数。不被 vue 管理的（定时器，ajax 回调函数，promise 回调函数）最好写成箭头函数。（没有完全理解）

##### v-on

- 阻止默认事件
  **js** 中的阻止默认事件

  ```html
  <a herf="https://......" @click="func">跳转</a>
  <script>
    methods:{
      func(event){
        event.preventDefault()
      }
    }
  </script>
  ```

  **vue** 中的阻止默认事件

  ```html
  <a herf="https://......" @click.prevent="func">跳转</a>
  <script>
    methods:{
      func(){
        ......
      }
    }
  </script>
  ```

- 阻止事件冒泡
  如果不用@click.stop，那么点击这个 a 标签将触发两次 func 函数，也就是事件冒泡。.stop 和.prevent 可以连用。

  ```html
  <div @click="func">
    <a herf="https://......" @click.stop="func">跳转</a>
    <a herf="https://......" @click.stop.prevent="func">跳转</a>
  </div>
  <script>
    methods:{
      func(){
        ......
      }
    }
  </script>
  ```

- 只触发一次事件
  `@click.once="func"`只有第一次点击标签会触发，后面再点就没用了。

- 键盘事件
  `@keyup.enter="showMessge"`点击回车触发。

##### class 样式的动态绑定

```css
.bg {
  background-color: red;
}
.fs {
  font-size: 30px;
}
```

- 字符串写法

  ```html
  <div :class="bg">小滴课堂</div>
  ```

- 对象写法

  ```html
  <div :class="obj">小滴课堂</div>
  obj:{bg: true, fs: true}
  ```

- 数组写法

  ```html
  <div :class="list">小滴课堂</div>
  list:['bg', 'fs'] //数组中是字符串
  ```

- 也可以搞点花样（下面两种写法等价）
  ```html
  <div :class="[trueOrFalse? classA:'', classB]" />
  <div :class="[{classA: trueOrFalse}, classB]" />
  ```

##### style 样式的动态绑定

不用 vue 的写法`<div style="font-size:30px; color:blue;" />`
用 vue 的写法`<div :style="{fontSize:'30px',color:'blue'}" />`，将 css 变成对象。

##### 列表渲染

- v-for 遍历数组
  使用 item in items 形式的特殊语法（也可以 in 换成 of），其中 items 是源数据数组，而 item 则是被迭代的数组元素的别名。
  `<li v-for="(item,index) in list">{{item.name}}-{{index}}</li>`

- 遍历对象
  三个参数为：值 键名 索引
  `<li v-for="(value,name,index) in obj">{{index}}：{{name}}：{{value}}</li>`

##### 维护状态 key 的作用和原理

`<li v-for="(item,index) in list" :key="index">{{item.name}}<input type="text" /></li>`

- 添加赵六进入数组`list.unshift({name:'赵六'})`。通过观察后方的 input 可以看出，前三个 li 标签没有变化，他们还是之前的 DOM 节点，只不过其中的内容变了。由第二张图可以看出，DOM 加入了新 li 节点 key=4，前三个节点没有变。
  ![](2024-03-02-00-38-21.png)
  ![](2024-03-02-00-44-34.png)
- 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key。改成下面的写法，给每个对象加一个 id，就能让 vue 明白每个节点的身份。
  `<li v-for="(item,index) in obj" :key="item.id">{{item.name}}</li>`
  ```js
  obj: [
      { name: '张三', id: '1' },
      { name: '李四', id: '2' },
      { name: '王五', id: '3' },
    ],
  ```
  - key 值使用 index，或者不加 key 值，在数组元素顺序打乱时，会产生不必要的 DOM 更新以及界面效果出问题。
  - key 主要用在 Vue 虚拟 DOM（类似 js 对象格式的数据） 的 Diff 算法，新旧虚拟 DOM 对比，复用不变的旧节点，渲染改变的节点，提高渲染速度

##### 数据的更新问题

- 通过普通对象添加属性方法，Vue 不能监测到且不是响应式。
  下面的例子中，obj.age 渲染不出来，应该使用以下两种方法。

  ```js
  {{obj.name}}  {{obj.age}}
  obj:{name:'wzt'}
  add(){
    this.obj.age= '23';   //Vue 监测不到
    this.$set(this.obj,'age','22'); //Vue 能监测到
    Vue.set(this.obj,'age','22');   //Vue 能监测到
  }
  ```

- 直接通过数组索引值改变数组的数据，Vue 监测不到改变。因为在 vue 中数组并没有跟对象一样封装有监测数据变化的 getter、setter。如果写成`this.list[0].name = 'zcl'`是可以的，但是写成`this.list[0] = { name: '李四',age: 20 };`vue 监测不到。
  Vue 在数组的原始操作方法（js 提供的）上包裹了重新解析模板的方法（vue 封装过的），让他们能被检测到。这 7 个函数如下所示。其他不改变原数组的函数就没必要封装了，例如 filter()。
  push() pop() shift() unshift() splice() sort() reverse()

##### 其他 v- 指令

- v-text `<p v-text="name">123</p>`
  有点类似{{}}，在所在节点渲染文本内容，但是会替换节点中现有的内容（例子中的 123）
- v-html `<p v-html="str"></p>`
  str 字符串里写的是 html，在所在节点渲染 html 结构的内容，替换节点所在的所有内容。在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 XSS 攻击（注入恶意指令代码到网页）。

##### vue cli

node_modules 项目的安装依赖
public 放置静态资源文件
src 项目的主入口文件夹
babel.config.js ES6 语法编译成 ES5 语法
package-lock.json 记录了当前项目所有模块的具体来源和版本号以及其他的信息
package.json 记录当前项目所依赖模块的版本信息

- main.js
  项目的主入口文件 src/main.js

  ```ts
  //根据传入的组件生成一个对应的vue实例
  import { createApp } from 'vue';
  //所有组件的父级组件
  import App from './App.vue';

  //将实例对象挂载到指定标签下
  createApp(App).mount('#app');
  ```

- App.vue
  项目中所有组件的父组件。

  ```ts
  <template>
    <!-- vue2组件html模板中必须有一个根标签<div/>，vue3中可以没有 -->
  </template>
  <script lang="ts">
  //defineComponent根据传入的配置对象，生成对应的组件
  import { defineComponent } from 'vue';
  //生成组件并暴露出去
  export default defineComponent({
    //组件名称
    name: 'App',
    //注册子组件
    components: {
      HelloWorld,
    },
  });
  </script>
  ```

- vue.config.js
  想要查看 vue-cli 的配置，可以通过`vue inspect > output.js`生成 output.js 文件。
  如果想要修改 vue-cli 的配置，可以新建 vue.config.js 文件，在其中修改配置。如何修改应该参考https://cli.vuejs.org/zh/config/

##### setup()

- 新的组件选项，在创建组件实例时，在 beforeCreate 之前执行(一次)。
- setup 方法是在 components, props, data, Methods, Computed, Lifecycle, methods 之前执行。
- 此组件对象还没有创建,this 是 undefined。
- 可以通过 getCurrentInstance 这个函数来返回当前组件的实例对象，也就是当前 vue 这个实例对象。`const {proxy}:any = getCurrentInstance();`

```ts
//如果在setup中返回值是一个对象，对象中的属性或方法，模版中可以直接使用
setup(){
  const msg:string='BJUT';
  return{
    msg
  }
}
```

&emsp;&emsp;setup()的返回值一般都返回一个对象：为模版提供数据，也就是模版中可以直接使用此对象中的所有属性方法。
返回对象中属性会与 data 函数返回对象的属性合并为组件对象的属性。返回对象中的方法会与 methods 中的方法合并成功组件对象的方法。

##### setup 的参数

- props：是一个对象，里面有父级组件向子级组件传递的数据，并且是在子级组件中使用 props 接收到的所有属性，并且获取到的数据将保持响应性。

  ```js
  export default {
    props: ['test'], //需要props声明才能在setup收到参数
    setup(props) {
      console.log(props.test);
    },
  };
  ```

- context：上下文对象，一般用来触发自定义事件。
  ```js
  export default {
    emits: ['event'], //需要emits声明才能在setup中使用
    setup(props, context) {
      function test() {
        context.emit('event', '123123');
      }
      return { test };
    },
  };
  ```
  attrs：它是绑定到组件中的 非 props 数据，并且是非响应式的。
  emit：vue2 中的 this.$emit();
  slot：是组件的插槽，同样也不是响应式的。

##### ref

ref 响应式类型是任意类型。这是它与 reactive 的明显区别。
基本类型数据的响应式是通过 Object.defineProperty() 实现
对象类型数据的响应式是通过 ES6 中的 Proxy 实现

```ts
//接受一个内部值并返回一个响应式且可变的 ref 对象。
const msg=ref('xdclass');
//通过 val.value方法返回的这个ref对象中的值
console.log(val.value);
//在模版中使用
//直接使用val即可，
<template>{{val}}<template/>
```

- 自动获取焦点

  ```ts

  <template>
    <input type="text"><br>
    <input type="text" ref="inputRef">
  </template>

  setup(){
      const inputRef=ref<HTMLElement|null>(null)
      onMounted(()=>{
          inputRef.value && inputRef.value.focus();
      })
      return{
          inputRef
      }
  }
  ```

##### reactive

reactive()：定义一个对象类型的响应式数据（不能处理基本类型数据）
`const proxy=reactive(obj)`接收一个普通对象然后返回该普通对象的响应式代理器对象。
​ 响应式转换是“深层的”：会影响对象内部所有的嵌套的属性。

- reactive 与 ref 的不同
  - 处理数据类型不同： ref 可以处理基本类型和对象（数组）类型数据，reactive 只能处理对象（数组）类型数据。
  - 实现原理不同：ref 处理基本类型数据通过 `Object.defineProperty()` 实现，reactive 通过 `Proxy` 实现。
  - 操作不同：ref 操作数据需要加 `.value`
  - 组件数据多时更加趋向使用 reactive。

##### toRef 与 toRefs

- toRef：创建一个 ref 对象，其 value 值指向另一个对象中指定的属性。用来将某个响应式对象的某一个属性提供给外部使用。
  `const name = toRef(person, "name");`或者
  ```js
  return{
    name: toRef(data,'name');
    age: toRef(data,'age');
  }
  ```
- toRefs：批量创建多个 ref 对象，其 value 值指向另一个对象中指定的属性
  ```js
  setup() {
    let person = reactive({
      name: "张三",
      age: 19,
    });
    return {  //扩展运算符将person对象中的所有属性提供给外部
      ...toRefs(person),
    };
  },
  ```

##### 生命周期钩子函数

创建---挂载---更新---销毁/卸载（vue2/vue3）
![](2024-03-03-00-23-48.png)

1. 如果在 beforeCreate()中 console.log(this)，得到的结果不一定是真实的。console.log(this)可能是在其他时期执行的。在后面写一个`debugger;`可以解决这个问题。
2. 在 beforeMount 阶段页面可以展示内容，但是是未编译。比如直接显示为`{{var}}`这个样子。此时可以操作 DOM，但是操作其实并未实际生效。因为操作会在虚拟 DOM 转为真实 DOM 的过程中被覆盖。
3. 如果没有 el 选项时，需要使用 vm.$mount(el)。el 选项就是下面这个东西。
   脚手架创建的项目的这行代码中，mount('#app')也是这个用途。`app.use(store).use(router).use(vant).mount('#app');`
   ```js
   var app = new Vue({
     el: '#app',
     data: {...},
   });
   ```

<img src="./2024-03-03-00-45-20.png" style="zoom:40%;"/>
<img src="./2024-03-03-00-48-02.png" style="zoom:40%;"/>

beforeDestroy 时，数据、方法可以访问但是不触发更新。

```js
setup(){  //vue3的生命周期函数写在setup中，比vue2的生命周期函数先执行
  //beforeCreate、created 合成了 setup()
  onBeforeMount(() => {});
  onMounted(() => {});
  onBeforeUpdate(() => {});
  onUpdated(() => {});
  onBeforeUnmount(() => {});
  onUnmounted(() => {});
}
```

##### computed

```ts
<template>
  <input type="text" v-model="first" /><br />
  <input type="text" v-model="second" /><br />
  <input type="text" v-model="addUp" /><br />
</template>
setup() {
    const first = ref<string>('');
    const second = ref<string>('');
    // const addUp = computed(() => {  两种写法皆可
    //   return first.value + second.value;
    // });
    const addUp = computed({
      get() {
        return first.value + second.value;
      },
      set(val) {
        first.value = val;
        second.value = val;
      },
    });
    return {first,second,addUp};
  },
```

当只读时可以简写

```ts
const addUp = computed(() => {
  return first.value + second.value;
});
```

计算属性优点：有缓存的机制，可以复用，效率高，调试方便。
有多个地方使用同一个计算属性，当值改变时，计算属性有缓存的机制，只会更新一次。

##### watch 监视器

```ts
const obj = reactive({
  name: 'Oscar',
  courses: ['ssm', 'javase', 'springboot'],
});
//watch:第一个参数监视源(可以是一个数组，表示监视多个)
//第二个参数回调函数
//第三个参数watch配置项(immediate和deep)
//其中，immediate表示obj在值在初始化的时候也执行回调函数
watch(
  obj, //也可以写成 'obj.name'，一定要写成字符串的形式，不然会报错
  (newValue, oldValue) => {
    console.log(newValue);
  },
  { immediate: true }
);
```

- _Vue 在监听整个 reactive 对象的 oldValue 的时候可能存在 bug_

- 简写
  ```js
  watch:{
    isSunny(){
      this.plan = this.isSunny ? 'go out' : 'stay home';
    },
  }
  ```
- 监听对象中的一个基本类型属性
  ```js
  watch(
    () => numObj.a, //写成回调函数
    (newValue, oldValue) => {
      console.log('numObj变化了', newValue, oldValue);
    }
  );
  ```
- 监听对象中的某几个属性
  ```js
  watch([() => obj.a, () => obj.b], (newValue, oldValue) => {
    console.log('obj变化了', newValue, oldValue);
  });
  ```
- 如果监听 reactive 定义的对象，深度监听是强制开启的，无法关闭（vue3 配置）

##### 对比 computed 与 watch

- vue 的作者认为 watch 会被滥用，监视属性是命令式且重复的。
- 计算属性实现更加简洁明了。因此，两者都能实现的功能，优先选择使用 computed。
- watch 能实现异步调用，computed 不能

例子：使用 watch 和 computed 实现用搜索框 inputValue 搜索列表 list

```js
list: [
  { name: '牛仔裤', price: '88元' },
  { name: '运动裤', price: '67元' },
  { name: '羽绒服', price: '128元' },
  { name: '运动服', price: '100元' },
],
computed: { //定义计算属性newList
  newList() {
    return this.list.filter((i) => {
      //indexOf() 用来查找的元素的位置
      //不包含inputValue的对象的indexOf结果为-1
      return i.name.indexOf(this.inputValue) !== -1;
    });
  },
},
watch:{
  inputValue:{
    immediate:true, //初始化组件时能够执行一次watch，
    //indexOf('')的结果为0，这样就能把整个列表都显示出来
    handler(value)=>{
      this.newList = this.list.filter((i)=>{
        return i.name.indexOf(value) !== -1
      })
    }
  }
}
```

##### watchEffect

watchEffect：在监听的回调函数中使用了属性，则监听该属性，不用在参数上指明监听哪个属性。

```js
watchEffect(() => {
  let var1 = numa.value;
  let var2 = numb.value;
  console.log('watchEffect函数执行了');
});
```

- 与 watch 的区别
  - watch 手动添加定向的监听属性
  - watchEffect 自动监听使用到的属性
  - watchEffect 会初始化执行一次，相当于`immediate:true`

建议开发中使用 watch 监听，逻辑简单、依赖属性少的场景可以使用 watchEffect

##### 组件基础

- 模块与组件
  模块：一般指一个 js 文件，提取公共或逻辑复杂的 js 代码。用来复用 js 代码、提高代码的复用率。
  模块化：当项目中的 js 都用模块来编写，那这个项目就是模块化的。
  组件：实现局部功能、逻辑的代码合集（html、css、js、image、map4）
  组件化：当项目中的功能或者页面都以组件的形式来去编写，那么这个项目就是组件化的。

- 组件的全局注册和局部注册
  - 全局注册
    data 必须是个函数，如果是个对象那么其他地方引用这个组件的时候会出问题。
    不写 el 选项（一个项目中所有的组件最终都由一个 vm 实例管理）
    ```js
    Vue.component('test-component', {
      data() {
        return {};
      },
      template: '...',
    });
    ```
  - 局部注册
    ```js
    const cmp = Vue.extend({
      data() {
        return {...};
      },
      template: '...',
    });
    new Vue({
      components: {cmp: cmp}
    })
    ```
- 父向子传值
  props 的数据时单向的，只能从父组件传到子组件
  props 的数据不可更改，如果要更改需备份到 data 中做操作.
  `<son-component :msg="123"/>`

  ```js
  props: {
    msg: {
      type: String,
      required: true,
      default:'你好',
    },
  },
  ```

- 子向父传值
  - 父组件通过 props 传给子组件事件回调传值
  ```js
  //父组件
  <son :transmit="transmit" />
  method:{
    transmit(i){
      this.xxx = i;
    }
  }
  //子组件
  props: ["transmit"],
  mounted() {
    this.transmit(this.var1);
  }
  ```
  - @绑定到自定义事件上
  ```js
  <son @event="handler" />
  method:{
    handler(i){
      this.xxx = i;
    }
  }
  //子组件
  mounted() {
    this.$emit('event',this.var1);
  }
  ```
  - 自定义事件（ref 绑定：灵活，延时效果）
  ```js
  // 父组件
  <son ref="child" />
  mounted() {
    this.$refs.child.$on("event", this.func1);//func1在methods声明或者用箭头函数
  },
  // 子组件
  methods: {
    func2() {
      this.$emit("event", this.var1);
    },
  },
  ```
  - 事件解绑
  ```js
  beforeDestroy(){
    this.$off()
  }
  ```
- 兄弟组件的数据操作
  将一个子组件的数据放在父组件维护（状态提升）。
  操作声明在父组件，传到另一个子组件就可实现兄弟组件间的数据操作。
- 任意组件的事件
  - 首先安装全局事件总线
  ```js
  new Vue({
    beforeCreate(){
      Vue.prototype.$bus = this
    }
    ...
  })
  ```
  - 在需要接收数据的组件绑定自定义事件`this.$bus.$on('xx',this.handler)`
  - 在提供数据的组件`this.$bus.$emit('xx',数据)`
  - 在绑定自定义事件的组件解绑`beforeDestroy() {this.$bus.$off();}`

##### slot 插槽

slot 是父子组件通讯的一种方式，可以在子组件指定的节点插入 html 内容。

- 默认插槽

  ```html
  <slotComponent>
    <span>123</span>
  </slotComponent>
  <!-- 子组件 -->
  <template>
    <div>
      <h1></h1>
      <p>
        <slot></slot>
      </p>
    </div>
  </template>
  ```

- 具名插槽：有多个 html 内容需要指定插入到子组件的对应节点

  ```html
  <slotComponent>
    <template v-slot:title>
      <span>123</span>
    </template>
    <template v-slot:website>
      <span>abc</span>
    </template>
  </slotComponent>
  <!-- 子组件 -->
  <template>
    <div>
      <h1>
        <slot name="title"></slot>
      </h1>
      <p>
        <slot name="website"></slot>
      </p>
    </div>
  </template>
  ```

  v-slot:title 只能写在 template 标签以及组件上。
  可以写成 `<span slot='title'>123</span>` 但是此用法在 vue3 被弃用。

- 作用域插槽：数据定义在子组件，但是数据需要在父组件的插槽中使用。
  ```html
  <slotComponent>
    <template v-slot="{ list }">
      <li v-for="(i, index) in list" :key="index">{{ i }}</li>
    </template>
  </slotComponent>
  <!-- 子组件 list是他data中的数据 -->
  <template>
    <div>
      <slot :list="list"></slot>
    </div>
  </template>
  ```

##### mixin 混入

- mixin 是一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项
- 局部混入
  ```js
  // 新建mixin.js文件
  export const myMixin = {
    data() {
      return {...};
    },
    mounted() {...},
  };
  // 需要混入的组件
  import { myMixin } from "../mixin";
  mixins: [myMixin],
  ```
- 全局混入 （谨慎使用，因为会使实例以及每个组件受影响）
  ```js
  // main.js
  import { myMixin } from './mixin';
  Vue.mixin(myMixin);
  ```
- 当组件和混入对象含有同名选项时，这些选项将进行“合并”。在选项发生冲突时以组件数据优先。

##### provide inject

用来在爷孙组件中传值

```ts
//爷爷组件
setup(){
  const color = ref<string>("yellow");
  provide("color",color);
  function changeColor(val:string){
    color.value = val;
  }
  return {color}
}
//孙子组件
setup(){
  const color = inject("color");
}
```

##### axios 配置

```ts
//axiosConfig.ts 文件
import axios from 'axios';
const http = axios.create({});
http.interceptors.request.use((req) => {
  return req;
});
http.interceptors.response.use((res) => {
  return res;
});
export default http;
```

```ts
//main.ts 文件
const app = createApp(App);
app.config.globalProperties.$http = http;
```

##### mockjs 设置

```ts
import Mock from 'mockjs';
//设置请求延时
Mock.setup({
  timeout: '200-2000',
});
//三个参数 正则表达式url, 请求方法, 请求的回调函数
Mock.mock(/\/api\/test/, 'get', (req: any) => {
  return {
    code: 0,
    data: {
      msg: '测试成功',
    },
  };
});
export default Mock;
```

- 在 main.ts 文件中，根据不同的 import 方式，有两种使用方法。这样 mock 才能被成功导入。

  ```ts
  import Mock from './mock/mockConfig';
  Mock;
  const app = createApp(App);
  ......
  ```

  ```ts
  import './mock/mockConfig';
  const app = createApp(App);
  ......
  ```

##### vue router

- 创建路由配置文件

  ```js
  // router/index.js
  import VueRouter from 'vue-router';
  import Home from '../components/Home';
  import Course from '../components/Course';​
  export default new VueRouter({
    routes: [{path: '/home', component: Home,},
      {path: '/course', component: Course,},
    ],
  });
  ```

  第二个例子：

  ```ts
  {
      path: '/home',
      name: 'Home',
      component: () => import('../components/Home/home.vue'),
      redirect: '/homeTop',
      children: [
        {
          path: '/homeTop',
          name: 'HomeTop',
          component: () => import('../components/Home/top.vue'),
        },
      ],
    },
  ```

- main.js 引入应用
  ```js
  import VueRouter from 'vue-router';
  import router from './router/index';
  ​
  Vue.use(VueRouter)​
  new Vue({
    ...
    router: router,
  });
  ```
- 展示路由
  `<router-view></router-view>` 有点像`<slot/>`
- 跳转

  - 标签跳转
    `<router-link active-class='active' to='/home'>首页<router-link>` 或者
    `<van-tabbar-item icon="home-o" to="homeTop">首页</van-tabbar-item>`
  - 编程式导航
    ```ts
    const { proxy }: any = getCurrentInstance();
    proxy.$router.push('/login');
    //push()和replace()中传入的参数就是标签跳转中to=""中的内容
    proxy.$router.push({ path: '/course/front', query: { text: text } });
    proxy.$router.replace({ path: '/course/front', query: { text: text } });
    ```

- 路由的前进后退
  replace:删除路由之前的历史记录
  `<router-link replace to="/course/back" active-class="active">`
  `proxy.$router.replace({ path: '/course/front', query: { text: text } });`

  ```js
  this.$router.forward(); //前进
  this.$router.back(); //后退
  this.$router.go(); //前进：正数1、2 或者后退：负数-1、-2
  ```

- 路由缓存
  跳转之后之前的组件就 destory 销毁了。路由缓存可以让不展示的路由组件保持挂载在页面，不被销毁。
  ```html
  <!-- componentXXX 是组件的名字，可以指定某一个组件不被销毁 -->
  <keep-alive include="componentXXX">
    <router-view></router-view>
  </keep-alive>
  ```
- 路由导航守卫 让用户必须登录
  例子 1 是简单写法，只能去登陆，其他地方不能去。例子 2 是高级写法，规定了那些地方不登陆也能去。

  ```ts
  router.beforeEach((to, from, next) => {
    const token = window.sessionStorage.getItem('token');
    if (to.path === '/login') {
      next();
    } else {
      if (token) {
        next();
      } else {
        next('/login');
      }
    }
  });
  ```

  ```js
  { //在路由配置中给页面加上`meta:{isAuth:true}`
    path: 'front',
    component: Front,
    meta: { isAuth: true },
  },
  router.beforeEach((to, from, next) => {
    if (to.meta.isAuth) {
      //同上，校验是否登陆
    } else {
      next();
    }
  });
  ```

##### 路由传参

- query 传参（对应 path 和?）

  ```html
  传字符串的时候，可以使用模版字符串
  <router-link :to="`/course/front?text=${text}`" />
  传对象的时候，写在query中
  <router-link :to="{ path: '/course/front', query: { text: text } }" />
  ```

  在子组件中获取`this.$route.query.text`

- params 传参（对应 name 和:）
  字符串形式传参时需加占位符告知路由器，在路径后面是参数
  ```js
  {
    name: 'qianduan',
    path: 'front/:text', //字符串形式传参时需加占位符告知路由器，在路径后面是参数
    component: Front,
  },
  ```
  ```html
  传字符串的时候，可以使用模版字符串
  <router-link :to="`/course/front/${text}`" />
  传对象的时候，写在params中
  <router-link :to="{ name: 'qianduan', params: { text: text } }" />
  ```
  在子组件中获取`this.$route.params.text`

##### vuex

Vuex：是集中式存储管理应用的所有组件的状态（数据），可实现任意组件之间的通讯。
特点： 1.当不同的组件需要对同一个状态进行读写时，或者复用的状态较多，可以使用 vuex。 2.能够保持数据和页面是响应式的 3.便于开发和后期数据维护

- store/index.js 文件
  ```js
  import Vuex from 'vuex'
  Vue.use(Vuex)            //应用
  ​
  export default new Vuex.Store({
    actions:{},            //异步，接受用户的事件
    mutations:{},          //只能通过mutations操作state中的数据
    state:{},              //存放共享的数据
    getters: {}            //对 state 数据进行加工
  })
  ```
  state 只能通过 mutations 配置的方法去修改
  mutations 必须是同步函数
  actions 能够提供 dispatch 方法实现异步操作。actions 中再调用 mutations 里的方法修改 state。
- 在组件中调用 vuex 中的方法
  `this.$store.dispatch("add", value); `

  ```ts
  import { useStore } from 'vuex';

  const store = useStore();
  function toCart(item: any) {
    store.commit('toCart', item);
    store.commit('getCart');
  }
  ```

- 在 vuex 中存储数据
  mutations 中的函数不会返回值，commit 方法并不会返回 mutations 中写的方法的返回值。它们只接受 state 和 payload（载荷，即传入的参数）作为参数，然后直接修改 state。

  ```ts
  state: {
      cartArray: [],
    },
  mutations: {  //添加商品到购物车
    toCart(state: any, tag: any) {
      const goods: any = state.cartArray.find((item: any) => item.id === tag.id);
      if (goods) {
        goods.count += 1;
      } else {
        const item = {
          id: tag.id,
          img: tag.img,
          title: tag.details,
          count: 1,
        };
        state.cartArray.push(item);
      }
    },  //查询商品
    getCart(state: any) {
      console.log(state.cartArray);
    },
  },
  ```

- getter：对 state 数据进行加工
  就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算
  ```ts
  getters: {  //cartArray是个复杂的对象数组，记录了购物车中的内容。getGoodsNum()用来加工出商品总数量
    getGoodsNum(state: any) {
      let num = 0;
      state.cartArray.forEach((item: any) => {
        num += item.count;
      });
      return num;
    },
  },
  ```

##### 打包配置

- vue.config.js
  ```js
  module.exports = {
    publicPath: './',
  };
  ```
- 修改路由为 hash 模式

  ```ts
  import { createRouter, createWebHashHistory } from 'vue-router';
  const router = createRouter({
    // history: createWebHistory(process.env.BASE_URL),
    history: createWebHashHistory(process.env.BASE_URL),
    routes,
  });
  ```

##### vscode 功能用户代码片段

功能用户代码片段是 vscode 中的一个功能。下列代码写在 vue.json 这一文件中，表示它只对 vue 文件生效。输入 prefix 中的变量，即 ts 可以生成下面一段代码。其中的“$1”“$2”代表按下tab后会切换到哪里。“${2:test}”代表选中 test。

```json
"Print to console": {
    "prefix": "ts",
    "body": [
    "<template>",
    "\t<div>",
    "$1",
    "\t</div>",
    "</template>",
    "<script lang=\"ts\">",
    "import {defineComponent} from 'vue';",
    "export default defineComponent({name: '${2:test}',})",
    "</script>",
    "<style scoped>",
    "</style>"
    ],
    "description": "Log output to console",
},
```
