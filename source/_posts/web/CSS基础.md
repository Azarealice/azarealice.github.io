---
title: CSS基础
date: 2024-01-30 16:10:02
categories: 前端
---

参考文章：https://developer.mozilla.org/zh-CN/docs/Web/CSS

### CSS语法

#### 基础

语法由一个 选择器（selector）起头。它选择了我们将要用来添加样式的 HTML 元素。在这个例子中我们为一级标题（主标题`<h1>` ）添加样式。

接着输入一对大括号 `{ }`。在大括号内部定义一个或多个形式为属性（property）—值（value）对的声明。每个声明都指定了我们所选择元素的一个属性，之后跟一个我们想赋给这个属性的值。

冒号之前是属性，冒号之后是值。不同的 CSS 属性对应不同的合法值。在这个例子中，我们指定了 `color` 属性，它可以接受许多颜色值；还有 `font-size` 属性，它可以接收许多 `size unit` 值。

```css
h1 {
  color: red;
  font-size: 5em;
  margin: 0;
  padding: 20px 0;
  text-shadow: 3px 3px 1px black;
}
```

#### 选择器

| 选择器                             | 效果                                                         |
| :--------------------------------- | ------------------------------------------------------------ |
| **类型、类和 ID 选择器**           |                                                              |
| `*`                                | 所有元素                                                     |
| `h1, h2`                           | 所有`h1`和`h2`元素                                           |
| `.special`                         | 所有`special`类元素                                          |
| `p.special.special2`               | 所有同时是`special`类和`special2`类的p元素                   |
| `#unique`                          | 单个id为`unique`的元素（多个元素不应使用相同id）             |
| **存否和值选择器**                 |                                                              |
| `a[title]`                         | 匹配带有一个名为`attr`的属性的元素——方括号里的值。           |
| `a[href="https://example.com"]`    | 匹配带有一个名为`attr`的属性的元素，其值正为`value`——引号中的字符串。 |
| ``p[class~="special"]``            | 匹配带有一个名为`attr`的属性的元素，其值正为`value`，或者匹配带有一个`attr`属性的元素，其值有一个或者更多，至少有一个和`value`匹配。注意，在一列中的好几个值，是用空格隔开的。 |
| `div[lang\|="zh"]`                 | 匹配带有一个名为`attr`的属性的元素，其值可正为`value`，或者开始为`value`，后面紧随着一个连字符`-`。 |
| **子字符串匹配选择器**             |                                                              |
| `li[class^="box-"]`                | 匹配带有一个名为`attr`的属性的元素，其值开头为`value`子字符串。 |
| `li[class$="-box"]`  | 匹配带有一个名为`attr`的属性的元素，其值结尾为`value`子字符串 |
| `li[class*="box"]` | 匹配带有一个名为`attr`的属性的元素，其值的字符串中的任何地方，至少出现了一次`value`子字符串。 |
| `li[class^="box-" i]` | 匹配带有一个名为`attr`的属性的元素，其值开头为`value`子字符串。<br>（忽略大小写） |
| **伪类和伪元素** |  |
| `a:visited` | 所有访问过的`a`元素 |
| `article p:first-child` | `article`元素内部的，第一个`p`元素 |
| `article p::first-line` | `article`元素内部的，所有`p`元素的第一行 |
| **关系选择器** |  |
| div p.special                      | 所有div元素内部的special类p元素                              |
| h1+p                               | 所有跟随在h1元素后面的p元素（兄弟元素）                      |
| div > p                            |                                                              |
|                                    |                                                              |
|                                    |                                                              |
|                                    |                                                              |
|                           |  |
|               |  |
|               |  |
|                       |                                                              |
| body h1+p .special                 | 在body内部的，在h1元素后面的p元素内部的，所有special类元素   |



### CSS引入

#### 外部样式表

外部样式表在一个单独的扩展名为 .css 的文件中包含 CSS。这是将 CSS 应用到文档中最常见和最有用的方法。你可以将一个 CSS 文件链接到多个网页上，用同一个 CSS 样式表为所有网页确定样式。

使用 HTML `<link>` 元素来链接外部样式表文件：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的 CSS 测试</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>这是我的第一个 CSS 示例</p>
  </body>
</html>
```

CSS 样式表文件可能如下所示：

```css
h1 {
  color: blue;
  background-color: yellow;
  border: 1px solid black;
}

p {
  color: red;
}
```

`<link>` 元素的 `href` 属性需要引用你文件系统中的一个文件。在上面的例子中，CSS 文件与 HTML 文档在同一个文件夹中，但你可以把它放在其他地方，并调整路径。这里有三个示例

```javascript
<!-- 在当前目录中，引用子文件夹 styles 中的样式表文件 -->
<link rel="stylesheet" href="styles/style.css" />

<!-- 在当前目录中，引用子文件夹 styles 中的子文件夹 general 中的样式表文件 -->
<link rel="stylesheet" href="styles/general/style.css" />

<!-- 在当前目录的父级目录中，引用子文件夹 styles 中的样式表文件 -->
<link rel="stylesheet" href="../styles/style.css" />
```

#### 内部样式表

一个内部样式表驻留在 HTML 文档内部。要创建一个内部样式表，你要把 CSS 放置在包含在 HTML `<head>` 元素中的 `<style>` 元素内。

一个内部样式表的 HTML 代码可能看起来像这样：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的 CSS 测试</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }
      p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>这是我的第一个 CSS 示例</p>
  </body>
</html>
```

在某些情况下，内部样式表可能是有用的。例如，也许你正在使用一个内容管理系统，其外部 CSS 文件是不可以直接修改的。

但对于有多个页面的网站来说，内部样式表就变成了一种不太有效的工作方式。要使用内部样式表在多个页面上应用统一的 CSS 样式，你必须在每个要使用该样式的网页上都有一个内部样式表。这种效率的下降也会影响到网站的维护。在内部样式表中使用 CSS，存在这样的风险：即使是一个简单的样式变化，也可能需要对多个网页进行编辑。

#### 内联样式

内联样式是影响单个 HTML 元素的 CSS 声明，包含在元素的 `style` 属性中。在一个 HTML 文档中，内联样式的实现可能看起来像这样：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的 CSS 测试</title>
  </head>
  <body>
    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">
      Hello World!
    </h1>
    <p style="color:red;">这是我的第一个 CSS 示例</p>
  </body>
</html>

```

**尽可能避免以这种方式使用 CSS**。这不符合最佳实践。首先，这是对 CSS 的维护效率最低的实现。一个样式的改变可能需要在一个网页中进行多次编辑。其次，内联 CSS 还将（CSS）表现性代码与 HTML 内容混合在一起，使一切都更难阅读和理解。将代码和内容分开，可以使所有从事网站工作的人更容易维护。

有几种情况下，内联样式是比较常见的。如果你的工作环境有很大的限制，你可能不得不使用内联样式。例如，也许你的内容管理系统（CMS）只允许你编辑 HTML 主体。你也可能在 HTML 电子邮件中看到大量的内联样式，以实现与尽可能多的电子邮件客户端的兼容。


### Footage
