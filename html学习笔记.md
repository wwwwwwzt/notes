# HTML 学习笔记

##### href 和 src 的区别

- herf

  - Hypertext Reference，表示超文本引用，**指向网络资源的所在位置**，用来建立当前文档和引用资源的联系。
  - 浏览器会识别该文档为 css 文档，并行下载该文档，并且不会停止对当前文档的处理，这也是在文档中不使用 @import 的原因。
    `<link href="./style.css" rel="stylesheet" />`
    `<a href="https://xdclass.net" />`

- src
  - 引用资源（js 脚本、图片），将目标资源**下载**应用到当前文档。
  - 当浏览器解析到该元素时，会暂停浏览器的渲染，直到该资源加载完毕，这也是 js 脚本放到 body 最下方的原因。
    `<script src="script.js"></script>`

##### css 的 link 标签放在头部，js 的 script 放在 body 底部

- link 标签放在 head 标签中
  - 用户访问网站时，代码是从上往下解析的，正常展示页面内容的样式，提高用户体验。这样浏览器一边下载 html 构建 DOM 树，一边下载 css 构建 css 树，然后两个合成为渲染树。
  - 放在 html 结构底部时，加载页面会出现 html 结构混乱的情况。
- script 标签放在 body 结束标签之前
  - JS 脚本在下载和执行期间会阻止 html 解析
  - 把 script 标签放在底部，保证 html 和 css 首先完成解析之后再加载 JS 脚本。
  - script 标签加上 defer（推迟的意思）属性时，可以放在 head 标签中 （async）

##### 提高搜索权重 SEO

- 详见大课笔记

- TDK：title、description、keywords
- 提高网站语义化的 html 标签占比
  `<a>、<p>、<ul>、<ol>、<li>、<h1>、<h2>、<h3>...`
  无语义化的标签`<div>、<span>`
- 服务端渲染

##### viewport

- 如何在不同移动设备的屏幕下正常展示网页的内容？
  考点：meta 标签 viewport 属性
  <image src="images/2024-03-15-01-07-33.png" style="zoom:30%;"/>
- 手机浏览器会把页面放入到一个虚拟的视口（viewpoint）中，但 viewport 又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域大，也可能比浏览器的可视区域小。通常这个虚拟的视口（viewport）比屏幕宽，会把网页挤到一个很小的窗口。
  `<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no" />`

##### DOM BOM

- DOM 就是⽂档对象模型，是⼀个抽象的概念。定义了访问和操作 HTML ⽂档的⽅法和属性。
- BOM 就是浏览器对象模型，内置对象定义操作浏览器的方法。
