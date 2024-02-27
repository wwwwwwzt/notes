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

##### setup()

&emsp;&emsp;新的组件选项，在创建组件实例时，在 beforeCreate 之前执行(一次)。setup 方法是在 components, props, data, Methods, Computed, Lifecycle, methods 之前执行。此组件对象还没有创建,this 是 undefined。可以通过 getCurrentInstance 这个函数来返回当前组件的实例对象，也就是当前 vue 这个实例对象。
`const {proxy}:any = getCurrentInstance();`

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

##### ref 自动获取焦点

ref 响应式类型是任意类型。这是它与 reactive 的明显区别。

```ts
//接受一个内部值并返回一个响应式且可变的 ref 对象。
const msg=ref('xdclass');
//通过 val.value方法返回的这个ref对象中的值
console.log(val.value);
//在模版中使用
//直接使用val即可，
<template>{{val}}<template/>
```

```ts
//自动获取焦点
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

`const proxy=reactive(obj)`接收一个普通对象然后返回该普通对象的响应式代理器对象。
​ 响应式转换是“深层的”：会影响对象内部所有的嵌套的属性。

##### setup 的参数

props：是一个对象，里面有父级组件向子级组件传递的数据，并且是在子级组件中使用 props 接收到的所有属性，并且获取到的数据将保持响应性。
context
attrs：它是绑定到组件中的 非 props 数据，并且是非响应式的。
emit：vue2 中的 this.$emit();
slot：是组件的插槽，同样也不是响应式的。

##### 生命周期钩子函数

```ts
setup(){  //vue3的生命周期函数写在setup中，比vue2的生命周期函数先执行
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

##### watch 监视器

```ts
const obj=reactive({
    name:'Oscar',
    courses:['ssm','javase','springboot']
})
//watch:第一个参数监视源(可以是一个数组，表示监视多个)
//第二个参数回调函数
//第三个参数watch配置项(immediate和deep)
watch(obj,(value),{immediate:true}=>{
    console.log(value);
})
```

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

- 路由导航守卫 让用户必须登录

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

- 使用路由引入组件

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

- 跳转方式

  ```html
  <van-tabbar-item icon="home-o" to="homeTop">首页</van-tabbar-item>
  ```

  ```ts
  const { proxy }: any = getCurrentInstance();
  proxy.$router.push('/login');
  ```

##### vuex

- 在组件中调用 vuex 中的方法

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

- getter
  就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算
  ```ts
  getGoodsNum(state: any) {
    let num = 0;
    state.cartArray.forEach((item: any) => {
      num += item.count;
    });
    return num;
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
