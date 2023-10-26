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

新的组件选项，在创建组件实例时，在 beforeCreate 之前执行(一次)。setup 方法是在 components, props, data, Methods, Computed, Lifecycle, methods 之前执行。

```ts
//如果在setup中返回值是一个对象，对象中的属性或方法，模版中可以直接使用
setup(){
  const msg:string='BJUT';
  return{
    msg
  }
}
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

##### Continue updating...

<div style="text-align:right;">2023/10/25 Oscar</div>
