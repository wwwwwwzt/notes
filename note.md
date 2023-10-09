# xdclass 学习笔记

## 第一期

### SEO(Search Engine Optimization) 搜索引擎优化

利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。
**黑帽 SEO**：通过欺骗技术和滥用搜索算法来推销毫不相关、主要以商业为着眼的网页。

- 关键字的堆叠
- 隐藏文本
- 门页（本身无意义，可以跳转到很多其他地方）

**白帽 SEO**：

- **TDK**，是指 seo 页面中的页面描述与关键词设置，"T"代表页头中的 title 元素，"D"代表页头中的 description 元素，"K"代表页头中的 keywords 元素。

```html
<meta
  name="keywords"
  content="小滴课堂，小D课堂,IT技能学习平台,在线教育,架构师,js教程，java教程，vue3教程，springboot教程，springcloud教程，vue教程，java开发，网页开发，html教程，微服务教程"
/>
<meta
  name="description"
  content="小滴课堂为IT技术人员终生学习提供最丰富的课程资源库,倾力分享了优质在线视频课程,几乎覆盖了IT技术的各个领域:java、js、vue、springboot、springcloud，涵盖前端、后端、运维、大数据、人工智能等，帮助每个渴望成长的IT技术工程师技能提升，学有所成！"
/>
```

- **提高网站语义化的 html 标签占比**
  - 搜索引擎的爬虫也依赖于 HTML 标记来确定上下文和各个关键字的权重，利于 SEO
  - `<a>、<p>、<ul>、<ol>、<li>、<h1>、<h2>、<h3>...`
  - 无语义化的标签`<div><span>`
- **SSR(Server Side Render)** 服务端渲染
  在服务器中运行 vue.js 生成 html 代码返回给浏览器渲染。

### SPA(single page web application)不能真正实现 SEO

- 在浏览器中查看 SPA 页面的源代码，发现没有显示页面中实际渲染的内容。这种由浏览器端的 js 主导渲染网页内容的方式，称之为客户端渲染。
- 在客户端生成 html，搜索引擎不会等待异步请求到数据返回给前端页面时再爬取。导致页面数据不会被搜索引擎正常收录到。
- 客户端渲染多用于强交互、不注重 SEO 的页面，例如：xx 管理系统、组件库类的项目(ElementUI)。
  <img src=2023-10-09-22-39-02.png style="zoom:40%;" />

### SSR 服务端渲染

- 由服务端生成 html 下发到浏览器直接渲染
- node 环境中使用 vue template 模版：nuxt 框架、renderToString api
- node 环境中使用 react jsx 模版：next 框架
  <img src=2023-10-09-22-52-04.png style="height:200px;width:300px;" />
- _第 3-5 课完全没看懂_。

[//]: # (Great notes. I've already learned a lot.)
