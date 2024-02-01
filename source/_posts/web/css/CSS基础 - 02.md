---
title: CSS基础 - 02
date: 2024-02-01 10:07:15
categories: 
- 前端
- CSS
---

参考文章：https://developer.mozilla.org/zh-CN/docs/Learn/CSS

## 盒模型

在 CSS 中我们广泛地使用两种“盒子”——**块级盒子**（block box）和**内联盒子**（inline box）。这两种盒子会在**页面流**（page flow）和元素之间的关系方面表现出不同的行为：

一个被定义成块级的（`block`）盒子会表现出以下行为：

- 盒子会在内联的方向上扩展并占据父容器在该方向上的所有可用空间，在绝大数情况下意味着盒子会和父容器一样宽
- 每个盒子都会换行
- `width` 和 `height` 属性可以发挥作用
- 内边距（`padding`）, 外边距（`margin`）和 边框（`border`）会将其他元素从当前盒子周围“推开”

除非特殊指定，诸如标题 (`<h1>`等) 和段落 (`<p>`) 默认情况下都是块级的盒子。

如果一个盒子对外显示为 `inline`，那么他的行为如下：

- 盒子不会产生换行。
- `width` 和 `height`属性将不起作用。
- 垂直方向的内边距、外边距以及边框会被应用但是不会把其他处于 `inline` 状态的盒子推开。
- 水平方向的内边距、外边距以及边框会被应用且会把其他处于 `inline` 状态的盒子推开。

用做链接的 `<a>` 元素、 `<span>`、 `<em>` 以及 `<strong>` 都是默认处于 `inline` 状态的。

我们通过对盒子`display`属性的设置，比如 `inline` 或者 `block` ，来控制盒子的外部显示类型。

完整的 CSS 盒模型应用于块级盒子，内联盒子只使用盒模型中定义的部分内容。模型定义了盒的每个部分——margin、border、padding 和 content——合在一起就可以创建我们在页面上看到的内容。为了增加一些额外的复杂性，有一个标准的和替代（IE）的盒模型。

### 盒模型的构成

CSS 中组成一个块级盒子需要：

- Content box: 这个区域是用来显示内容，大小可以通过设置 `width` 和 `height`.
- Padding box: 包围在内容区域外部的空白区域；大小通过 `padding` 相关属性设置。
- Border box: 边框盒包裹内容和内边距。大小通过 `border` 相关属性设置。
- Margin box: 这是最外面的区域，是盒子和其他元素之间的空白区域。大小通过 `margin` 相关属性设置。

如下图：

![Diagram of the box model](https://s2.loli.net/2024/02/01/lL1TXSEZY5UtRA9.png)

### 标准盒模型

在 CSS 盒子模型的默认定义里，你对一个元素所设置的 `width` 与 `height` 只会应用到这个元素的内容区。如果这个元素有任何的 `border` 或 `padding` ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点尤其烦人。

`box-sizing` 属性可以被用来调整这些表现：

- `content-box` 是默认值。如果你设置一个元素的宽为 100px，那么这个元素的内容区会有 100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。
- `border-box` 告诉浏览器：你想要设置的边框和内边距的值是包含在 width 内的。也就是说，如果你将一个元素的 width 设为 100px，那么这 100px 会包含它的 border 和 padding，内容区的实际宽度是 width 减去 (border + padding) 的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。

如果你希望所有元素都使用替代模式，而且确实很常用，设置 `box-sizing` 在 `<html>` 元素上，然后设置所有元素继承该属性，正如下面的例子。

```css
html {
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}
```

### 外边距折叠

理解外边距的一个关键是外边距折叠的概念。如果你有两个外边距相接的元素，这些外边距将合并为一个外边距，即最大的单个外边距的大小。

在下面的例子中，我们有两个段落。顶部段落的页 `margin-bottom`为 50px。第二段的`margin-top` 为 30px。因为外边距折叠的概念，所以框之间的实际外边距是 50px，而不是两个外边距的总和。

```css
.one {
  margin-bottom: 50px;
}
.two {
  margin-top: 30px;
}
```

```html
<div class="container">
  <p class="one">I am paragraph one.</p>
  <p class="two">I am paragraph two.</p>
</div>
```

### 盒子模型和内联盒子

以上所有的方法都完全适用于块级盒子。有些属性也可以应用于内联盒子，例如由`<span>`元素创建的那些内联盒子。

在下面的示例中，我们在一个段落中使用了`<span>`，并对其应用了宽度、高度、边距、边框和内边距。可以看到，宽度和高度被忽略了。外边距、内边距和边框是生效的，但它们不会改变其他内容与内联盒子的关系，因此内边距和边框会与段落中的其他单词重叠。

![image-20240201132254312](https://s2.loli.net/2024/02/01/XHUMPRS4CqcIwf7.png)

```css
span {
  margin: 20px;
  padding: 20px;
  width: 80px;
  height: 50px;
  background-color: lightblue;
  border: 2px solid blue;
}
```

```html
<p>
  I am a paragraph and this is a <span>span</span> inside that paragraph. A span is an inline element and so does not respect width and height.
</p>
```

### 使用 display: inline-block

display 有一个特殊的值，它在内联和块之间提供了一个中间状态。这对于以下情况非常有用：你不希望一个项切换到新行，但希望它可以设定宽度和高度，并避免上面看到的重叠。

一个元素使用 `display: inline-block`，实现我们需要的块级的部分效果：

- 设置`width` 和`height` 属性会生效。
- `padding`, `margin`, 以及`border` 会推开其他元素。

但是，它不会跳转到新行，如果显式添加 `width` 和 `height` 属性，它只会变得比其内容更大。

**在下一个示例中，我们将 `display: inline-block` 添加到 `<span>` 元素中。**

![image-20240201132449409](https://s2.loli.net/2024/02/01/JbvENuBP8lHTAVr.png)

```css
span {
  margin: 20px;
  padding: 20px;
  width: 80px;
  height: 50px;
  background-color: lightblue;
  border: 2px solid blue;
  display: inline-block;
}
```

```html
<p>
  I am a paragraph and this is a <span>span</span> inside that paragraph. A span is an inline element and so does not respect width and height.
</p>
```

当你想要通过添加内边距使链接具有更大的命中区域时，这是很有用的。`<a>` 是像 `<span>` 一样的内联元素；你可以使用 `display: inline-block` 来设置内边距，让用户更容易点击链接。
