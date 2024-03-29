---
title: CSS基础 - 01
date: 2024-01-30 16:10:02
categories: 
- 前端
- CSS
---

参考文章：https://developer.mozilla.org/zh-CN/docs/Learn/CSS

## 语法

### 基础

语法由一个 选择器（`selector`）起头。它选择了我们将要用来添加样式的 HTML 元素。在这个例子中我们为一级标题（主标题`<h1>` ）添加样式。

接着输入一对大括号 `{ }`。在大括号内部定义一个或多个形式为属性（`property`）—值（`value`）对的声明。每个声明都指定了我们所选择元素的一个属性，之后跟一个我们想赋给这个属性的值。

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

### 选择器

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
| `div p.special`                    | 所有`div`元素内部的`special`类`p`元素                        |
| `div > p` | 所有`div`元素内部的直接子`p`元素 |
| `h1 + p`                           | 直接跟随在`h1`元素后面的`p`元素（兄弟元素）                |
| `h1 ~ p`              | 所有跟随在`h1`元素后面的`p`元素（兄弟元素） |

## 引入

### 外部样式表

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

### 内部样式表

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

### 内联样式

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

## 优先级

应用于元素的每个 CSS 属性，只能有一个值。你可以通过在浏览器的开发者工具中检查元素来查看应用于元素的所有属性值。工具的“样式”面板显示了应用在被检查元素上的所有属性值，以及匹配的选择器和 CSS 源文件。具有优先权的来源（origin）的选择器将其值应用于匹配的元素。

除了应用的样式外，“样式”面板还显示了与选定元素匹配但由于层叠、优先级或源顺序而未应用的被划掉的值。划掉的样式可能来自具有相同优先权（precedence）来源但具有较低优先级（specificity），或者来源和优先级相同，但在代码库中较早出现。对于任何应用的属性值，可能有来自许多不同来源的几个声明被划掉。如果你看到一个划掉的样式，它有一个具有更高优先级的选择器，这意味着该值的来源或重要性较低。

通常情况下，随着网站复杂度的增加，样式表数量也会增加，这使得样式表的源顺序变得更加重要和复杂。层叠层简化了在这样的代码库维护样式表的复杂程度。层叠层是显式优先级容器，提供了对最终应用的 CSS 声明更简单和更强大的控制能力，使 web 开发者能够在不必与优先级抗争的情况下，优先考虑 CSS 的部分。

CSS （Cascading Style Sheets）中的 C 代表“层叠”。这是样式层叠在一起的方法。用户代理经过几个非常明确定义的步骤来确定每个元素的每个属性的值。我们将在这里简要列出这些步骤：

- **相关声明**：找到所有具有匹配每个元素的选择器的声明代码块。
- **重要性**：根据规则是普通还是重要对规则进行排序。重要的样式是指设置了 `!important` 标志的样式。
- **来源**：在两个按重要性划分的分组内，按作者、用户或用户代理这几个来源对规则进行排序。
- **层叠层**：在六个按重要性和来源划分的分组内，按层叠层进行排序。普通声明的层顺序是从创建的第一个到最后一个，然后是未分层的普通样式。对于重要的样式，这个顺序是反转的，但保持未分层的重要样式优先权最低。
- **优先级**：对于来源层中优先权相同的竞争样式，按优先级对声明进行排序。
- **出现顺序**：当两个来源层的优先权相同的选择器具有相同的优先级时，最后声明的具有最高优先级的选择器的属性值获胜。

**对于每一步，只有“仍在运行”的声明才会进入下一轮“竞争”。如果只有一个声明在运行，那么它就“赢了”，后续的步骤就没有意义了。**

### 来源优先级

有三种层叠来源类型：**用户代理样式表**、**用户样式表**和**作者样式表**。浏览器根据来源和重要性将每个声明分为六个来源分组。有八个优先权级别：六个来源分组、正在过渡的属性和正在动画的属性。优先权的顺序是从具有最低优先权的普通用户代理样式，到当前应用的动画中的样式，到具有最高优先权的重要用户代理样式，再到正在过渡的样式：

- 用户代理普通样式
- 用户普通样式
- 作者普通样式
- 正在动画的样式
- 作者重要样式
- 用户重要样式
- 用户代理重要样式
- 正在过渡的样式

**“用户代理”指的是浏览器。“用户”指的是是网站访问者。“作者”指的是你，开发者**。用 `<style>` 元素直接在元素上声明的样式是作者样式。不包括动画和过渡样式，用户代理普通样式具有最低优先权；用户代理重要样式具有最高优先权。

来源优先权总是胜过选择器优先级。如果一个元素属性被多个来源中的普通样式声明所设置，那么作者样式表将总是覆盖用户或用户代理样式表中声明的冗余普通属性。如果样式是重要的，那么用户代理样式表将总是胜过作者和用户样式。层叠来源优先权确保了不同来源之间的优先级冲突永远不会发生。

### 层叠优先级

类似于我们有六个基于来源和重要性的优先权级别，层叠层使我们能够在这些来源中创建子来源级别的优先权。

层的优先权始终高于选择器的优先级。具有优先权的层中的样式“胜出”于具有较低优先权的层。输掉的层中选择器的优先级是无关紧要的。在层内竞争的属性值的优先级仍然会受到注意，但是在层之间没有优先级的问题，因为每个属性对应的最高优先权层才会被考虑。

#### 层叠层的创建

可以使用以下任一方法创建层叠层：

- 使用 `@layer` 声明 at 规则，使用 `@layer` 后跟一个或多个层的名称来声明层。这将创建一个没有分配任何样式的具名层。
- 使用 `@layer` 块 at 规则，在块中的所有样式都将添加到一个命名或未命名的层中。
- 使用具有 `layer` 关键字或 `layer()` 函数的 `@import`规则，将导入文件的内容分配到该层中。

在尚未初始化具有相同名称的层的情况下，这三种方法中的任何一种都会创建一个层。如果在 `@layer` at 规则或带有 `layer()` 的 `@import` 中没有提供层名称，则将创建一个新的匿名层。

层的顺序由 CSS 中层出现的顺序确定。使用 `@layer` 后跟一个或多个层的名称而不分配任何样式是定义层顺序的一种方式。

`@layer` CSS at 规则用于声明层叠层，并在存在多个层叠层时定义优先权顺序。以下规则按照列出的顺序声明了三个层：

```css
@layer theme，layout，utilities;
```

通常，你需要在 CSS 的第一行声明这个 `@layer`（当然要用对你的站点有意义的层名称），以便完全控制层的顺序。

如果上述声明是站点 CSS 的第一行，那么层的顺序将是 `theme`、`layout` 和 `utilities`。如果在上述语句之前已经创建了一些层，只要同名的层还不存在，这三个层就会被创建并添加到现有层列表的末尾。但是，如果同名的层已经存在，那么上述语句只会创建两个新层。例如，如果 `layout` 已经存在，只会创建 `theme` 和 `utilities`, 但在这种情况下的层顺序将是 `layout`、`theme` 和 `utilities`。

#### 层叠层的排序

可以使用块 `@layer` at 规则来创建层。如果一个 `@layer` at 规则后跟一个标识符和一个块样式，则该标识符用于命名该层，该规则中的样式被添加到该层的样式中。如果具有指定名称的层不存在，则会创建一个新层。如果具有指定名称的层已经存在，则会将样式添加到先前存在的层中。如果在使用 `@layer` 创建块样式时没有指定名称，则该规则中的样式将被添加到一个新的匿名层中。

在下面的示例中，我们使用了四个块和一个内联的 `@layer` at 规则。这个 CSS 按列出的顺序执行以下操作：

1. 创建一个命名的 `layout` 层
2. 创建一个未命名的匿名层
3. 声明三个层的列表并只创建两个新层 `theme` 和 `utilities`，因为 `layout` 已经存在
4. 向已经存在的 `layout` 层添加额外的样式
5. 创建第二个未命名的匿名层

```css
/* 文件：layers1.css */

/* 未分层的样式 */
body {
  color: #333;
}

/* 创建第一个层：`layout` */
@layer layout {
  main {
    display: grid;
  }
}

/* 创建第二个层：一个未命名的匿名层 */
@layer {
  body {
    margin: 0;
  }
}

/* 创建第三和第四个层：`theme` 和 `utilities` */
@layer theme，layout，utilities；
/* 向已经存在的 `layout` 层添加样式 */
@layer layout {
  main {
    color: #000;
  }
}

/* 创建第五个层：一个未命名的匿名层 */
@layer {
  body {
    margin: 1vw;
  }
}

```

在上面的 CSS 中，我们创建了五个层：`layout`、`<anonymous(01)>`、`theme`、`utilities` 和 `<anonymous(02)>`——按这个顺序——第六个隐含的未分层样式层包含在 `body` 样式块中。层的顺序是层的创建顺序，未分层样式的隐含层总是在最后的。一旦创建了层之后就无法改变层的顺序。

我们将一些样式分配给名为 `layout` 的层。如果指定的具名层不存在，则在 `@layer` at 规则中指定名称（无论是否向层分配样式）都会创建该层；这会将该层添加到现有层名称系列的末尾。如果指定的具名层已经存在，该命名块内的所有样式都会附加到之前存在的层中的样式——通过重用现有的层名称指定样式不会创建新层。

通过在不命名层的情况下将样式分配给层来创建匿名层。只能在创建时向未命名的层添加样式。

#### 层叠层的优先级

```css
@import url(A.css) layer(firstLayer);
@import url(B.css) layer(secondLayer);
@import url(C.css);
```

上述代码创建了两个具名层和一个未命名层。假设这三个文件（`A.css`、 `B.css` 和 `C.css`）本身不包含任何额外的层。以下列表显示了在这些文件内外声明的样式将以从最低（1）优先权到最高（10）优先权进行排序。

1. firstLayer 普通样式（`A.css`）
2. secondLayer 普通样式（`B.css`）
3. 未分层普通样式（`C.css`）
4. 内联普通样式
5. 动画样式
6. 未分层重要样式（`C.css`）
7. secondLayer 重要样式（`B.css`）
8. firstLayer 重要样式（`A.css`）
9. 内联重要样式
10. 过渡样式

在层中声明的普通样式具有最低的优先权，并按照创建层的顺序进行排序。在最先创建的层中声明的普通样式具有最低的优先权，而在最后创建的层中声明的普通样式在所有层中具有最高的优先权。换句话说，如果存在冲突的话，在 `firstLayer` 中声明的普通样式将被列表中任何后续的样式覆盖。

接下来是在层外声明的任何样式。`C.css` 中的样式没有导入到层中，并将覆盖任何来自 `firstLayer` 和 `secondLayer` 的冲突样式。在层外声明的普通样式总是比层内的普通样式具有更高的优先权。

内联样式是使用 `style`属性声明的。以这种方式声明的普通内联样式将优先于在未分层和分层样式表中找到的普通样式（`firstLayer - A.css`，`secondLayer - B.css` 和 `C.css`）。

动画样式比所有普通样式都具有更高的优先权，包括内联普通样式。

重要样式，即包含 `!important` 标志的属性值，优先于我们列表中前面提到的任何样式。它们的排序顺序与普通样式的顺序相反。在层外声明的任何重要样式的优先权都低于在层内声明的样式。在层中找到的重要样式也按层的创建顺序进行排序。对于重要样式，最后创建的层具有最低的优先权，而首先创建的层在声明的层中具有最高的优先权。

内联重要样式再次优先于在其他地方声明的重要样式。

过渡样式具有最高的优先权。当正在过渡普通属性值时，它优先于所有其他属性值声明，甚至是内联重要样式；但是只在过渡时。

总结：

- 层的优先权顺序是创建层的顺序。
- 一旦创建，就无法更改层顺序。
- 普通样式的层优先权是创建层的顺序。
- 未分层普通样式优先于有层普通样式。
- 重要样式的层优先权被反转，早期创建的层具有优先权。
- 所有有层的重要样式都优先于未分层的重要（和普通）样式。
- 普通内联样式优先于所有普通样式，无论是否分层。
- 重要内联样式优先于所有其他样式，正在过渡的样式除外。
- 作者样式无法覆盖重要内联样式（过渡除外，但这是临时的）。

### 选择器优先级

你会发现在一些情况下，有些规则在最后出现，但是却应用了前面的具有冲突的规则。这是因为前面的有更高的优先级——它范围更小，因此浏览器就把它选择为元素的样式。

浏览器是根据优先级来决定当多个规则有不同选择器对应相同的元素的时候需要使用哪个规则。它基本上是一个衡量选择器具体选择哪些区域的尺度：

- 一个元素选择器不是很具体，则会选择页面上该类型的所有元素，所以它的优先级就会低一些。
- 一个类选择器稍微具体点，则会选择该页面中有特定 `class` 属性值的元素，所以它的优先级就要高一点。

下面我们再来介绍两个适用于 `<h1>` 的规则。下面的 `<h1 class="main-heading">` 最后会显示红色——类选择器 `main-heading` 有更高的优先级，因此就会被应用——即使元素选择器顺序在它后面。

```css
.main-heading {
  color: red;
}
h1 {
  color: blue;
}
```

一个选择器的优先级可以说是由三个不同的值（或分量）相加，可以认为是百（ID）十（类）个（元素）——三位数的三个位数：

- ID：选择器中包含 ID 选择器则百位得一分。
- 类：选择器中包含类选择器、属性选择器或者伪类则十位得一分。
- 元素：选择器中包含元素、伪元素选择器则个位得一分。

## 继承

继承需要在上下文中去理解，一些设置在父元素上的 CSS 属性是可以被子元素继承的，有些则不能。

举一个例子，如果你设置一个元素的 `color` 和 `font-family`，每个在里面的元素也都会有相同的属性，除非你直接在元素上设置属性。

一些属性是不能继承的——举个例子如果你在一个元素上设置 `width` 为 50% ，所有的后代不会是父元素的宽度的 50% 。如果这个也可以继承的话，CSS 就会很难使用了！

每个CSS 属性定义的概述都指出了这个属性是默认继承的 (`Inherited: Yes`) 还是默认不继承的 (`Inherited: no`)。这决定了当你没有为元素的属性指定值时该如何计算值。

CSS 为控制继承提供了五个特殊的通用属性值。每个 CSS 属性都接收这些值。

- `inherit`
  设置该属性会使子元素属性和父元素相同。实际上，就是“开启继承”。
- `initial`
  将应用于选定元素的属性值设置为该属性的初始值。
- `revert`
  将应用于选定元素的属性值重置为浏览器的默认样式，而不是应用于该属性的默认值。在许多情况下，此值的作用类似于 `unset`。
- `revert-layer`
  将应用于选定元素的属性值重置为在上一个层叠层中建立的值。
- `unset`
  将属性重置为自然值，也就是如果属性是自然继承那么就是 `inherit`，否则和 `initial` 一样
