# xdclass 学习笔记

## 第一期

### SEO(Search Engine Optimization) 搜索引擎优化
利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。
**黑帽SEO**：通过欺骗技术和滥用搜索算法来推销毫不相关、主要以商业为着眼的网页。
- 关键字的堆叠
- 隐藏文本
- 门页（本身无意义，可以跳转到很多其他地方）
  
**白帽SEO**：
- **TDK**，是指seo页面中的页面描述与关键词设置，"T"代表页头中的title元素，"D"代表页头中的description元素，"K"代表页头中的keywords元素。
``` html
<meta name="keywords" content="小滴课堂，小D课堂,IT技能学习平台,在线教育,架构师,js教程，java教程，vue3教程，springboot教程，springcloud教程，vue教程，java开发，网页开发，html教程，微服务教程">
<meta name="description" content="小滴课堂为IT技术人员终生学习提供最丰富的课程资源库,倾力分享了优质在线视频课程,几乎覆盖了IT技术的各个领域:java、js、vue、springboot、springcloud，涵盖前端、后端、运维、大数据、人工智能等，帮助每个渴望成长的IT技术工程师技能提升，学有所成！">
```
- **提高网站语义化的html标签占比** 
  - 搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO
  - `<a>、<p>、<ul>、<ol>、<li>、<h1>、<h2>、<h3>...`
  - 无语义化的标签`<div><span>`
- **SSR(Server Side Render)** 服务端渲染
在服务器中运行vue.js生成html代码返回给浏览器渲染。
[//]: # (Great notes. I've already learned a lot.)
