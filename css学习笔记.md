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

##### BFC 块级格式化上下文

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
