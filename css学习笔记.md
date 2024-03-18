# css 学习笔记

##### css 选择器以及优先级

内联样式 > id 选择器 > 类选择器 = 属性选择器 = 伪类选择器 > 标签选择器 = 伪元素选择器
`<div style="..." id="a" class="b" test=1 />`
内联样式：标签里自带的 style
id 选择器：#a{}
类选择器：.b{} 属性选择器 div[test=1]{} 伪类选择器.b:hover{}
标签选择器：div{} 伪元素选择器.b::before{}

- 组合选择符

  ```html
  <div class="a">
    <div class="b">
      <div class="c" />
      <div class="d" />
    </div>
  </div>
  ```

  - 后代选择符 .a .c{} //可以隔着 b 这一代设置 c
  - 子代选择符 .a > .b{} //不可以隔着 b 这一代
  - 相邻选择符 .c + .d{} //c 旁边的 d
  - 计算 id 选择器的个数 a。计算类选择器、属性选择器、伪类选择器的个数 b。计算标签选择器、伪元素选择器的个数 c。对比 a、b、c 的个数，相等则比较下一个。

- 属性后面加 !important 拥有最高优先级，若两个样式都有此设置，则对比选择器优先级

##### 盒子模型

<image src="images/2024-03-15-18-03-22.png" style="zoom:25%;" />

- 标准盒模型：宽高包含 content + padding + border。content 就是 268x182。
- 怪异盒模型：宽高只包含 content。content 小于 268x182，被 border 和 padding 抢走了空间。

```css
box-sizing:content-box // 标准盒模型，默认
box-sizing:border-box  // 怪异盒模型
```

##### BFC 块级格式化上下文 Block Formatting Context

BFC 是页面盒模型布局中的一种 CSS 渲染模式，相当于一个独立的容器，里面的元素和外部的元素相互不影响。

- 内部的盒子会按照垂直方向一个个排列
- 同一个 BFC 下的相邻块级元素会发生外边距折叠，创建新的 BFC 包含其中一个元素可以避免（解决外边距重叠）

  ```html
  <!-- 他们两个的实际间距为40px，而不是（40+20=）60px。将父元素设置成BFC可以解决这一问题 -->
  <div style="overflow:hidden">
    <div style="margin-bottom:40px"></div>
    <div style="margin-top:20px"></div>
  </div>
  ```

- 设置了 BFC 的区域不会和浮动元素重叠（解决浮动元素覆盖）
  ```html
  <!-- 此时下面的元素会覆盖上面的元素。将父元素设置成BFC可以使他们左右排列 -->
  <div style="overflow:hidden">
    <div style="height:40px;width:40px;float:left"></div>
    <div style="height:40px;width:40px;"></div>
  </div>
  ```
- 当 BFC 中有浮动元素时，该浮动元素的高度也会被计算其中（解决高度塌陷）
  ```html
  <!-- 如果没有float，父元素的高度等于子元素撑开的高度。如果没有float，父元素会高度为0 -->
  <!-- 将父元素开启BFC，父元素的高度又等于子元素的高度。 -->
  <div style="overflow:hidden">
    <div style="height:40px;width:40px;float:left"></div>
  </div>
  ```
- 触发 BFC
  - 设置 float 浮动
  - overflow 的值是 hidden（**最常用**）、auto 或者 scroll，而不是 visible
  - position 的值为 absolute 或 fixed
  - display:table | inline-block | flex | grid

##### flex

```css
/* ​父元素设置了flex布局，默认会给每个子元素开启缩小属性（flex-shrink:1;）
当空间不够时，其他的元素会被挤压至隐藏。
通过设置flex-shrink:0;来避免。*/
display:flex

/* 定义水平方向对齐方式 */
justify-content: flex-start | flex-end | center | space-between | space-around;
​
/* 定义垂直方向对齐方式 */
align-items: flex-start | flex-end | center | baseline | stretch;
​
/* 定义多个轴线（多行/多列）对齐方式 */
align-content: flex-start | flex-end | center | space-between | space-around | stretch;

/* 设置子元素 */
flex:1 代表着
flex-grow:1; //允许放大
flex-shrink:1; //允许缩小
flex-basis:0%;
```

##### CSS 实现三栏布局的几种方式

- flex 布局
  ```html
  <div style="display: flex">
    <div style="height: 50px; width: 50px;"></div>
    <div style="flex: 1; height: 50px; width: 50px;"></div>
    <div style="height: 50px; width: 50px;"></div>
  </div>
  ```
- 浮动+margin
  左右 float 中间设置等于左右宽度的 margin，自己不设 width。
  ```html
  <div>
    <div style="float:left; height: 50px; width: 50px;"></div>
    <div style="margin:0 50px; height: 50px;"></div>
    <div style="float:left; height: 50px; width: 50px;"></div>
  </div>
  ```
- 浮动+BFC
  左右 float 中间开启 BFC。
  ```html
  <div>
    <div style="float:left; height: 50px; width: 50px;"></div>
    <div style="overflow:hidden; height: 50px; width: 50px;"></div>
    <div style="float:left; height: 50px; width: 50px;"></div>
  </div>
  ```

##### CSS 中的预处理器

less\sass\stylus

- 什么是预处理器？

  - 定义了专门的编程语言，增加了编程的特性，生成 CSS 文件
  - CSS 代码更加简洁、适应性更强、可读性更佳，更易于代码的维护等

- 预处理器的能力
  - 嵌套反映层级和约束
  - 变量和计算减少重复代码
  - extend 和 mixin 代码片段
  - 循环适用于复杂有规律的样式
  - import css 文件模块化

##### 让盒子水平垂直居中

```html
<div class="a">
  <div class="b"></div>
</div>
```

- 利用 flex 弹性盒子
  ```css
  .a{   //父元素设置flex和居中
    display:flex
    justify-content:center; //水平
    align-items:center;  //垂直
  }
  ```
- position+margin
  ```css
  .a {
    ......
    position: relative;
  }
  .b {
    ......
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
  }
  ```
- position
  ```css
  .a {
    ......
    position: relative;
  }
  .b {
    ......
    position: absolute;
    top: 50%;
    left: 50%;  //此时，b类元素的左上角在正中间，但需要把中心移到最中间
    transform: translate(-50%, -50%);
  }
  ```

##### css 移动端的适配

- 安装 postcss-pxtorem，自动将 px 转换成 rem 单位的插件。px 是一个绝对值，会让页面在不同移动端设备中很不一样。rem 是相对值，可以缓解这一问题。
- 安装 amfe-flexible，自动检测当前设备屏幕宽度 serveWidth，设置 html 里面的 font-size 为 serveWidth/10

##### 重绘和重排

![]()<image src="images/2024-03-18-10-31-47.png" style="zoom:40%;"/>

- 重绘(repaint)：当元素的外观、背景、颜色等改变，浏览器会根据元素的新属性重新绘制，使元素呈现新的外观叫做重绘。
- 重排(reflow)：当渲染树一部分或者全部因为大小或者边距而改变，需要渲染树重新计算的过程叫做重排。
- 重绘不一定需要重排，重排必然导致重绘。
- 避免：
  - 在元素的显示隐藏上尽量用 opacity 替代 visibility（重绘）
  - 元素定位时使用 transform 代替 top、left（重排）
  - 尽量不使用 table 布局，因为一个小的改动会造成整个 table 重新布局（重排）
  - 减少直接操作 DOM 元素（重排）
  - 为元素添加类，样式都在类中改变（重绘）
