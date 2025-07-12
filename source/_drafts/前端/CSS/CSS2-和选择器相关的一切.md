## 一、基础选择器
### 1. 通配选择器
**作用**：选择页面中的所有元素。  
**语法**：
```css
* {
    margin: 0;
    padding: 0;
}
```

### 2. 类型选择器
**作用**：选择指定的HTML标签。  
**语法**：
```css
p {
    font-size: 16px;
}
```

### 3. 类选择器
**作用**：选中具有特定class属性的元素，可复用。  
**语法**：
```css
.box {
    background-color: yellow;
}
```

### 4. ID选择器
**作用**：选中具有特定id属性的元素（唯一）。  
**语法**：
```css
#header {
    color: red;
}
```

### 5. 分组选择器
**作用**：对多个选择器应用相同样式，使用逗号分隔。  
**语法**：
```css
h1, h2, h3 {
    font-family: sans-serif;
}
```

## 二、组合选择器
### 1. 后代选择器
**作用**：选中某元素内部的所有指定后代元素。  
**语法**：
```css
A B {
    /* 样式 */
}

```
**示例：**
选择器 `.container p` 表示：在 class="container" 元素里面的所有 `<p>` 标签，不管它们是直接子元素还是嵌套在几层里面。
```css
.container p {
  color: red;
}
```

```html
<div class="container">
  <p>这是第一段文字</p>
  <div>
    <p>这是第二段文字</p>
  </div>
</div>
```
### 2. 子代选择器
**作用**：只选中直接子元素。  
**语法**：
```css
A > B {
  /* 样式 */
}
```
**示例：**
第一个 `<p>` 是 `.parent` 的直接子元素，会变红。
第二个 `<p>` 是嵌套在 `div` 里面的，只是 `.parent` 的“孙子”，不会变红。
```css
.parent > p {
  color: red;
}
```

```html
<div class="parent">
  <p>直接子元素</p>
  <div>
    <p>孙子元素</p>
  </div>
</div>
```

### 3. 相邻兄弟选择器
**作用**：选中紧接着某元素之后的第一个兄弟元素。  
**语法**：
```css
A + B {
  /* 样式 */
}
```
**示例：**
第一个 `<p>` 是 `<h2>` 的相邻兄弟，会变成红色。
第二个 `<p>` 虽然也是兄弟，但 不紧挨 h2，所以不会被选中。
```css
h2 + p {
    color: green;
}
```

```html
<h2>标题</h2>
<p>这是紧挨着 h2 后的段落</p>
<p>这是第二个段落</p>
```

### 4. 通用兄弟选择器
**作用**：选中某元素后**所有**兄弟元素。  
**语法**：
```css
A ~ B {
  /* 样式 */
}
```
**示例：**
选中 `<h2>` 后面的所有 `<p>` 元素（只要是兄弟关系）
所以三个 `<p>` 全都会变蓝
```css
h2 ~ p {
    font-style: italic;
}
```

```html
<h2>我是标题</h2>
<p>第一个段落</p>
<p>第二个段落</p>
<div>不是段落</div>
<p>第三个段落</p>
```

## 三、属性选择器
属性选择器（Attribute Selectors）是 CSS 中用于根据 HTML 元素的属性及其值进行样式匹配的一种选择器方式。它可以选中带有特定属性的元素，而不需要使用类或 ID。

**基本语法格式：**
```css
[属性名] {
  /* 样式 */
}
[属性名="值"] {
  /* 样式 */
}
```

**常见属性选择器及示例：**

| 选择器               | 说明                           | 示例选中目标                                  |
| ----------------- | ---------------------------- | --------------------------------------- |
| `[attr]`          | 选中带有 `attr` 属性的所有元素          | `[type]` → 所有有 `type` 属性的元素             |
| `[attr="value"]`  | 选中 `attr` 属性等于 `"value"` 的元素 | `[type="text"]` → 所有 `type="text"` 的输入框 |
| `[attr~="value"]` | `attr` 的值中包含**以空格分隔**的某个词    | `[class~="btn"]` → 含 `btn` 的 class      |
| `[attr^="value"]` | `attr` 的值以 `"value"` 开头      | `[href^="https"]` → https 开头的链接         |
| `[attr$="value"]` | `attr` 的值以 `"value"` 结尾      | `[src$=".jpg"]` → 后缀是 `.jpg` 的图片        |
| `[attr*="value"]` | `attr` 的值中包含 `"value"`（任意位置） | `[title*="提示"]` → 包含“提示”二字的 title 属性    |

**示例代码**
```html
<input type="text" placeholder="请输入内容">
<input type="password">
<a href="https://example.com">链接</a>
<img src="cat.jpg" alt="一只猫">
<img src="cat.phg" alt="一只猫">
<p class="btn primary">按钮</p>
<p class="primary">按钮</p>
```

```css
/* 选中所有 type 为 text 的 input */
input[type="text"] {
  border: 2px solid blue;
}

/* 选中所有 href 以 https 开头的链接 */
a[href^="https"] {
  color: green;
}

/* 选中所有 src 以 .jpg 结尾的图片 */
img[src$=".jpg"] {
  border-radius: 10px;
}

/* 选中 class 中包含 btn 的元素 */
[class~="btn"] {
  background-color: orange;
}
```

## 四、伪类选择器
伪类选择器（Pseudo-classes）是 CSS 中用来选择 特定状态下的元素 的选择器，例如：鼠标悬停、元素是第一个子元素、被点击过的链接等等。
**语法:**
```css
selector:pseudo-class {
    /* 样式 */
}
```

**常见伪类选择器一览：**

|伪类|说明|示例|
|---|---|---|
|`:hover`|鼠标悬停在元素上时|`a:hover`|
|`:active`|元素被点击时（激活状态）|`button:active`|
|`:focus`|元素获得焦点时（比如输入框被点中）|`input:focus`|
|`:visited`|已访问过的链接|`a:visited`|
|`:first-child`|元素是其父元素的第一个子元素时|`li:first-child`|
|`:last-child`|元素是其父元素的最后一个子元素时|`li:last-child`|
|`:nth-child(n)`|选中父元素中第 n 个子元素（从 1 开始计数）|`li:nth-child(2)`|
|`:nth-of-type(n)`|选中同类型中的第 n 个元素|`p:nth-of-type(1)`|
|`:not(selector)`|选中不是某个选择器的元素|`div:not(.hidden)`|
|`:checked`|表单中被选中的复选框或单选框|`input:checked`|
|`:disabled` / `:enabled`|选中禁用 / 可用状态的表单元素|`input:disabled`|
|`:empty`|没有子元素（包括文字节点）的元素|`div:empty`|

**示例代码**
1.鼠标悬停时变色
```html
<a href="#">悬停我</a>
```

```css
a:hover {
    color: red;
    text-decoration: underline;
}
```

2.选中第一个子元素设置样式
```html
<ul>
  <li>第一个</li>
  <li>第二个</li>
</ul>
```

```css
li:first-child {
    font-weight: bold;
}
```

3.给已选中的复选框加样式
```html
<input type="checkbox" checked>
```

```css
input:checked {
    outline: 2px solid green;
}
```

4.选中除了特定类名外的所有元素
```html
<div class="box">展示</div>
<div class="hidden">隐藏</div>
```

```css
div:not(.hidden) {
    border: 1px solid black;
}
```

## 五、伪元素选择器
伪元素选择器（Pseudo-elements）是 CSS 中用来 创建虚拟的元素节点，这些节点并不实际存在于 HTML 结构中，但你可以像操作真实元素那样为它们设置样式。

**语法：**
```css
selector::pseudo-element {
    /* 样式 */
}
```

**常见伪元素选择器：**

|伪元素|作用|
|---|---|
|`::before`|在元素 **内容前** 插入一个虚拟元素|
|`::after`|在元素 **内容后** 插入一个虚拟元素|
|`::first-line`|选中文本的 **第一行**，通常用于段落等|
|`::first-letter`|选中文本的 **第一个字母**，用于装饰首字母|
|`::selection`|选中文本时（鼠标拖选或Ctrl+A），设置其背景色、文字色等样式|
|`::marker`|用于列表项的项目符号（如 `ul` 的点），可自定义符号样式（较新）|
|`::placeholder`|输入框中的占位符文字样式|
**示例：**
1.使用 `::before` 插入内容
```html
<p class="note">注意事项</p>
```

```css
.note::before {
    content: "⚠️ ";
    color: red;
}
```
说明：虚拟插入了一个“⚠️”符号，不影响原始 HTML 结构。

2.使用 `::after` 加尾部标记
```html
<a href="#">点击查看</a>
```

```css
a::after {
    content: " 🔗";
}
```
说明：在链接后添加一个链接符号“🔗”。

 3.装饰首字母
```css
p::first-letter {
    font-size: 2em;
    color: red;
}
```
 说明：段落第一个字变大变红，适合用于文章首字美化。
