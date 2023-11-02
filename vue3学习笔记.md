# vue3 学习笔记

##### main.js

项目的主入口文件 src/main.js

```ts
//根据传入的组件生成一个对应的vue实例
import { createApp } from 'vue';
//所有组件的父级组件
import App from './App.vue';

//将实例对象挂载到指定标签下
createApp(App).mount('#app');
```

##### App.vue

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

##### setup() (没太明白)

&emsp;&emsp;新的组件选项，在创建组件实例时，在 beforeCreate 之前执行(一次)。setup 方法是在 components, props, data, Methods, Computed, Lifecycle, methods 之前执行。此组件对象还没有创建,this 是 undefined。可以通过 getCurrentInstance 这个函数来返回当前组件的实例对象，也就是当前 vue 这个实例对象。`const {proxy}:any = getCurrentInstance();`

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

##### ref

```ts
//接受一个内部值并返回一个响应式且可变的 ref 对象。
const msg=ref('xdclass');
//通过 val.value方法返回的这个ref对象中的值
console.log(val.value);
//在模版中使用
//直接使用val即可，
<template>{{val}}<template/>
```

##### reactive

`const proxy=reactive(obj)`接收一个普通对象然后返回该普通对象的响应式代理器对象。
​ 响应式转换是“深层的”：会影响对象内部所有的嵌套的属性。

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

##### Continue updating...

<div style="text-align:right;">2023/10/25 Oscar</div>
