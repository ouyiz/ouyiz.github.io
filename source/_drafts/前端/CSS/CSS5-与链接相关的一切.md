## 一、a 标签的基本作用
HTML 中的 `<a>` 标签用于创建超链接。通过 CSS，我们可以控制链接的 **颜色、样式、行为状态、图标等**。

## 二、链接的四种伪类状态
CSS 提供了四个伪类来设置链接在不同交互状态下的样式

| 伪类         | 状态说明        | 示例          | 常见样式控制             |
| ---------- | ----------- | ----------- | ------------------ |
| `:link`    | 未访问的链接      | `a:link`    | 字体颜色、下划线           |
| `:visited` | 已访问的链接      | `a:visited` | 改变颜色但**不能改背景/边框等** |
| `:hover`   | 鼠标悬停在链接上    | `a:hover`   | 改颜色、添加动画、改变下划线等    |
| `:active`  | 链接点击时（按下瞬间） | `a:active`  | 改变颜色、缩放等           |

**建议总是按 `:link → :visited → :hover → :active` 顺序写**，否则 hover 样式可能被覆盖。

**示例：**
```css
a:link {
  color: blue;
  text-decoration: underline;
}
a:visited {
  color: purple;
}
a:hover {
  color: red;
  text-decoration: none;
}
a:active {
  color: orange;
}
```

## 三、文本装饰相关属性

| 属性名                             | 作用描述                |
| ------------------------------- | ------------------- |
| `text-decoration`               | 快速设置文本的装饰线（综合写法）    |
| `text-decoration-line`          | 指定添加哪种装饰线，如下划线、删除线等 |
| `text-decoration-style`         | 设置装饰线的样式，如实线、虚线等    |
| `text-decoration-color`         | 设置装饰线的颜色            |
| `text-underline-offset`         | 设置下划线与文本的距离         |
| `text-decoration-thickness`（较新） | 控制装饰线的粗细            |

### 1. `text-decoration` —— 快速设置文本装饰
```css
selector {
    text-decoration: underline dotted red;
}
```
- 合成属性，相当于同时设置：
    ```css
    text-decoration-line: underline;
    text-decoration-style: dotted;
    text-decoration-color: red;
    ```

### 2. `text-decoration-line` —— 设置装饰线类型
```css
p {
  text-decoration-line: underline overline;
}
```
**取值**：
- `none`：无装饰
- `underline`：下划线
- `overline`：上划线
- `line-through`：删除线

### 3. `text-decoration-style` —— 设置装饰线的样式
```css
p {
  text-decoration-line: underline;
  text-decoration-style: wavy;
}
```
**取值**：
- `solid`：实线（默认）
- `double`：双线
- `dotted`：点状线
- `dashed`：虚线
- `wavy`：波浪线

### 4. `text-decoration-color` —— 设置装饰线颜色
```css
p {
  text-decoration: underline;
  text-decoration-color: #e74c3c;
}
```
- 支持所有 CSS 颜色值，如：
    - 色名（red）
    - 十六进制（#3498db）
    - rgb / rgba（rgba(0, 0, 0, 0.5)）

### 5. `text-underline-offset` —— 下划线偏移量（与文本间距）
```css
p {
  text-decoration: underline;
  text-underline-offset: 4px;
}
```
**取值**
- `length`：如 `2px`、`0.2em`
- `auto`：自动（默认）

### 6. `text-decoration-thickness` —— 装饰线粗细（较新）
```css
p {
  text-decoration: underline;
  text-decoration-thickness: 2px;
}
```
**取值**：
- `auto`
- 长度值（如 `1px`, `0.1em`）

## 四、指针样式（cursor）
改变鼠标悬停在链接上时的光标样式：
```css
a {
  cursor: pointer;
}
```
常用鼠标样式

|鼠标样式值|效果说明|
|---|---|
|`default`|默认箭头指针|
|`pointer`|手型，通常用于链接或按钮|
|`text`|文本输入型（I 型光标）|
|`move`|表示元素可移动|
|`wait`|表示程序正在处理（小圆圈或沙漏）|
|`help`|问号光标，表示有帮助信息|
|`not-allowed`|禁止符号，表示不允许操作|
|`progress`|表示操作正在进行（可以点击）|
|`crosshair`|十字准星，用于图形工具等|
|`grab`|抓手形状，表示可拖拽|
|`grabbing`|抓取中的样式|

## 五、超链接布局样式
### 1.为什么要设置链接的布局样式？
默认情况下：
```html
<a href="#">超链接</a>
```
`<a>` 是 **行内元素（inline）**，有这些限制：
- **不能设置宽度、高度**
- **内外边距效果有限**
- **点击区域只覆盖文字本身**
这就不利于“交互性设计”和“可访问性”——尤其在移动端，用户需要更大的可点击区域。

### 2.相关 CSS 属性详解
`display` —— 设置显示类型

|属性值|说明|
|---|---|
|`inline`|默认，行内元素，不可设置宽高等|
|`block`|块级元素，单独一行，可设置宽高|
|`inline-block`|**结合前两者**，不换行又可设置布局属性|
|`flex` / `grid`|更强大布局方式，适用于高级导航栏设计|
```css
a {
  display: inline-block;
  padding: 10px;
}
```


## 七、链接的安全性控制相关属性
- **rel 属性**
    - `noopener`：防止打开的页面控制当前页面。
    - `noreferrer`：不传递引用来源。
    - 通常搭配 `target="_blank"` 使用：
        ```html
        <a href="..." target="_blank" rel="noopener noreferrer">安全链接</a>
        ```


## 七、伪元素与图标结合链接美化
 ::before / ::after 添加图标或符号
	- 伪元素 `::before` 和 `::after` 是**给元素“前面”或“后面”**加内容的方式，而**不改变 HTML 结构本身**。
	- 下述例子将在链接开头添加一个“🔗”图标。
```CSS
 a::before {
  content: "🔗 ";
}

.with-icon::before {
  content: "🔗 ";
  color: tomato;
}
```
## 八、禁用链接点击
虽然 HTML 没有真正禁用 `<a>` 的方式，但可以通过 CSS 模拟：
```css
a.disabled {
  pointer-events: none;
  color: gray;
  cursor: not-allowed;
}
```

## 九、现代链接效果（动效、变换、过渡）
在 CSS 中，`transition` 属性用于定义元素在状态变化时的过渡效果。
具体来说，`transition` 属性的语法是：
```css
transition: property duration timing-function delay;
```
- **property**：你希望应用过渡效果的 CSS 属性（如 `color`, `background-color`, `transform` 等）。
    - `color`：文本颜色
    - `background-color`：背景颜色
    - `opacity`：透明度
    - `transform`：变形（例如旋转、平移）
    - `width`、`height`：宽高
    - `left`、`top`：定位属性
    - `text-decoration`：文本装饰（如下划线、删除线等）
- **duration**：过渡持续的时间，例如 `0.3s`（3秒）或者 `0.5s`。
- **timing-function**：指定过渡的速度曲线。
    - `linear`：匀速变化。
    - `ease`：默认值，先慢后快，再慢。
    - `ease-in`：渐变开始时慢，结束时快。
    - `ease-out`：渐变开始时快，结束时慢。
    - `ease-in-out`：渐变开始和结束时都慢，中间快。
    - `cubic-bezier(x1, y1, x2, y2)`：自定义的贝塞尔曲线。
- **delay**：过渡开始前的延迟时间，例如 `1s`（1秒）。
**示例：**
平滑过渡效果（使用 `transition`）
```css
a {
  transition: color 0.3s ease, text-decoration 0.3s ease, background-color 0.3s ease;
}
```

点击放大效果（使用 `transform`）
```css
a:active {
  transform: scale(0.95);
}
```

背景颜色渐变效果
```css
a {
  transition: background-color 0.5s ease-in-out;
}

a:hover {
  background-color: #3498db;
  color: white;
}
```

边框变化与文本装饰
```css
a {
  transition: border 0.3s ease, text-decoration 0.3s ease;
  border-bottom: 2px solid transparent;
}

a:hover {
  border-bottom: 2px solid #3498db; /* 添加底部边框 */
  text-decoration: underline; /* 增加下划线 */
}
```

实现文本缩放效果
```css
a {
  transition: transform 0.3s ease;
}

a:hover {
  transform: scale(1.2); /* 放大为原来的 1.2 倍 */
}
```


## 十.本节代码
**代码**
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>超链接 CSS 样式大全</title>
  <!-- FontAwesome 图标支持 -->
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" rel="stylesheet">
  <style>
    /* 一、链接四种状态 */
    /* 针对页面中 .status-demo 类下的所有链接（<a> 标签）的样式设置 */
    .status-demo a:link {
      color: #2ecc71; /* 未访问的链接 */
      text-decoration: underline;
    }
    .status-demo a:visited {
        color: #e74c3c; /* 修改访问后的链接颜色 */
    }
    .status-demo a:hover {
        color: #f1c40f; /* 鼠标悬停时，链接颜色 */
    }
    .status-demo a:active {
        color: #3498db; /* 点击时，链接颜色 */
    }

    /* 二、文本装饰相关属性 */
    .text-demo{
        text-decoration-line: underline; /* 文本装饰线：下划线 */
        text-decoration-style: dashed; /* 文本装饰样式：虚线 */
        text-decoration-color: #f1c40f; /* 装饰线颜色 */
        text-decoration-thickness: 2px; /* 装饰线粗细 */
    }

    /* 三、鼠标指针样式 */
    .cursor-default{
        cursor: default; /* 默认鼠标指针 */
    }
    .cursor-pointer{
        cursor: pointer; /* 手型鼠标指针 */
    }
    .cursor-move{
        cursor: move; /* 移动鼠标指针 */
    }
    .cursor-not-allowed{
        cursor: not-allowed; /* 禁止鼠标指针 */
    }
    .cursor-help{
        cursor: help; /* 帮助鼠标指针 */
    }
    .cursor-wait{
        cursor: wait; /* 等待鼠标指针 */
    }
    .cursor-crosshair{
        cursor: crosshair; /* 十字鼠标指针 */
    }

    /*四、超链接布局样式 */
    .display-block{
        display: block; /* 块级元素 */
    }
    .display-inline{
        display: inline; /* 内联元素 */
    }
    .display-inline-block{
        display: inline-block; /* 内联块级元素 */
        width: 100px; /* 设置元素宽度 */
        height: 100px; /* 设置元素高度 */
        background-color:  #3498db;; /* 设置元素背景色 */
        color: white; /* 设置元素字体颜色 */
        line-height: 100px;      /* 与高度一致实现垂直居中 */
        text-align: center;      /* 水平居中 */
        text-decoration-line: none; /* 去掉下划线 */
    }
    .display-none{
        display: none; /* 隐藏元素 */
    }

     /* 五、安全链接 rel */
     .rel-demo a {
        color: #2c3e50;
        text-decoration: underline;
    }

    /* 六、超链接图标 */
    .with-icon::before {
        content: "🔗 ";
        color: tomato;
    }

     /* 七、更多现代链接效果 */
     .modern-link {
        color: #2c3e50;
        position: relative;
        text-decoration: none;
    }

    .modern-link::before {
        content: "";
        position: absolute;
        bottom: -5px;
        left: 0;
        width: 100%;
        height: 2px;
        background: #e74c3c;
        transform: scaleX(0);
        transform-origin: bottom right;
        transition: transform 0.3s ease;
    }

    .modern-link:hover::before {
        transform: scaleX(1);
        transform-origin: bottom left;
    }

    .modern-link:hover {
        color: #e74c3c;
    }

    .hover-scale-link {
        display: inline-block;
        transition: transform 0.3s ease;
    }

    .hover-scale-link:hover {
        transform: scale(2); /* 放大效果 */
    }

    .hover-opacity-link {
        transition: opacity 0.3s ease-in-out;
    }

    .hover-opacity-link:hover {
        opacity: 0.5; /* 透明度效果 */
    }

    .hover-bg-color-link {
        transition: background-color 0.3s, color 0.3s;
    }

    .hover-bg-color-link:hover {
        background-color: #3498db;
        color: white; /* 背景颜色和文本颜色变化 */
    }

    .hover-rotate-link {
        transition: transform 0.3s ease;
    }

    .hover-rotate-link:hover {
        display: inline-block;
        transform: rotate(10deg); /* 旋转效果 */
    }

    
  </style>

</head>
<body>
    <div class="status-demo">
        <h2>一.链接四种状态</h2>
        <a href="https://www.bing.com">访问bing</a>
    </div>
    <div>
        <h2>二.文本装饰相关属性</h2>
        <a href="https://www.bing.com" class="text-demo">这是一个带有文本装饰的段落。</a>
    </div>
    <div>
        <h2>三.鼠标指针样式</h2>
        <a href="https://www.bing.com" class="cursor-default">默认鼠标指针</a><br>
        <a href="https://www.bing.com" class="cursor-pointer">手型鼠标指针</a><br>
        <a href="https://www.bing.com" class="cursor-move">移动鼠标指针</a><br>
        <a href="https://www.bing.com" class="cursor-not-allowed">禁止鼠标指针</a><br>
        <a href="https://www.bing.com" class="cursor-help">帮助鼠标指针</a><br>
        <a href="https://www.bing.com" class="cursor-wait">等待鼠标指针</a><br>
        <a href="https://www.bing.com" class="cursor-crosshair">十字鼠标指针</a><br>
    </div>
    <div>
        <h2>四.超链接布局样式</h2>
        <a href="https://www.bing.com" class="display-block">块级元素</a><br>
        <a href="https://www.bing.com" class="display-inline">内联元素</a><br>
        <a href="https://www.bing.com" class="display-inline-block">内联块级元素</a>
        <a href="https://www.bing.com" class="display-inline-block">内联块级元素</a>
        <a href="https://www.bing.com" class="display-inline-block">内联块级元素</a><br>
        <a href="https://www.bing.com" class="display-none">隐藏元素</a><br>
    </div>

    <div class="rel-demo">
        <h2>五.安全链接 rel</h2>
        <a href="https://www.bing.com" target="_blank" rel="noopener noreferrer">安全链接</a><br>
    </div>

    <div>
        <h2>六.超链接图标</h2>
        <a href="https://www.bing.com" class="with-icon">这是一个带有图标的链接</a>
    </div>

    <div>
        <h2>七.多现代链接效果 </h2>
        <a href="https://www.bing.com" class="modern-link">现代链接</a><br><br>
        <a href="https://www.bing.com" class="hover-scale-link">放大效果</a><br><br>
        <a href="https://www.bing.com" class="hover-opacity-link">透明度效果</a><br><br>
        <a href="https://www.bing.com" class="hover-bg-color-link">背景颜色变化</a><br><br>
        <a href="https://www.bing.com" class="hover-rotate-link">旋转效果</a><br>
    </div>
    </div>

</body>
</html>
```

**参考图**
![](file-20250423204437844.png)