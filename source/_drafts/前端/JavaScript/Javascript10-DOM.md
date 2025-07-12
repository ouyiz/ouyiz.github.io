## 一、什么是 DOM？
**DOM（文档对象模型，Document Object Model）** 是一种用来表示 HTML 或 XML 文档的结构的标准。
在浏览器里：
- 网页加载后，浏览器会把 HTML 文本转换成 DOM 树结构。
- 每一个 HTML 元素（如 `<div>`、`<p>`、`<img>`）都会变成树上的一个节点。
- JavaScript 可以通过 DOM 操作网页的内容、结构和样式。
![](file-20250530133521638.png)
例如你写了一个 HTML 页面：
```html
<body>
  <h1>标题</h1>
  <p>内容段落</p>
</body>
```
浏览器会把它变成一个“DOM 树”，JavaScript 就能访问这些元素，像这样：
```js
document.querySelector("h1").textContent = "新标题";
```

## 二、如何访问 DOM 元素
DOM 元素是通过 `document` 对象来访问的，`document` 代表整个网页文档。

#### 1.`getElementById("id")` — 根据 `id` 获取元素
```html
<p id="demo">1</p>
<p id="demo">2</p>
```

```js
const el = document.getElementById("demo");
console.log(el.textContent); // 输出：1（只返回第一个）
```
用于查找某一个明确的唯一元素。
- HTML 规范中规定：一个页面中每个 `id` 应该是唯一的。
- 所以 `getElementById("...")` 永远只会返回一个元素对象（或者找不到时返回 `null`），不会返回数组或列表。
- 如果你页面中不小心写了两个相同的 `id`，浏览器仍然只会返回第一个匹配的那个。

#### 2️.`getElementsByClassName("class")` — 根据类名查找元素
```html
<div class="box"></div>
<div class="box"></div>
```

```js
const boxes = document.getElementsByClassName("box");
```
返回的是一个 **HTMLCollection**（伪数组），可以用 `for` 或 `for...of` 遍历。

#### 3.`getElementsByTagName("tag")` — 根据标签名获取元素
```js
const allDivs = document.getElementsByTagName("div");
```
返回页面中所有指定标签的元素集合，跟 `getElementsByClassName` 类似。

#### 4.`querySelector("选择器")` — 使用 CSS 选择器语法获取第一个匹配的元素
```html
<div id="container" class="box">
  <p class="msg">Hello</p>
  <p class="msg">World</p>
</div>
```

```js
document.querySelector("p");        // 选择第一个 <p>
document.querySelector(".msg");     // 选择第一个 class 为 msg 的元素
document.querySelector("#container"); // 选择 id 为 container 的元素
document.querySelector("div > p");  // 选择 div 下的第一个 p
```
使用 CSS 语法写法（如 `.class`, `#id`, `div > p`），返回第一个符合条件的元素。

#### 5.`querySelectorAll("选择器")` — 获取所有匹配的元素（NodeList）
```js
const allBoxes = document.querySelectorAll(".box");
```
返回 NodeList，可以用 `forEach()` 遍历，功能比 `getElementsByClassName` 更强大。

## 三、操作 DOM 内容和属性

#### 1. 修改文本或 HTML 内容
```html
<p id="demo">旧内容</p>
<script>
  const element = document.getElementById("demo");
  element.textContent = "新内容";//设置纯文本，变为<p id="demo">新内容</p>
  element.innerHTML = "<b>加粗</b>";//设置HTML内容，变为<p id="demo"><b>加粗</b></p>
</script>
```
`textContent` 设置纯文本，不会被解析成 HTML。  
`innerHTML` 会被解析为 HTML，如果插入 `<script>` 有安全风险。

#### 2. 修改属性（如 `href`, `src`, `id`）
```html
<a id="myLink" href="#">原链接</a>
<script>
  const link = document.getElementById("myLink");

  link.setAttribute("href", "https://openai.com"); // 设置 href 属性
  const href = link.getAttribute("href");          // 获取当前值
  console.log(href); // 输出： https://openai.com
</script>
```
`.setAttribute()` 用于设置任意属性  
`.getAttribute()` 用于获取属性的值

#### 3.也可以直接通过对象属性访问：
```html
<img id="myImg" />
<input id="myInput" />

<script>
  const img = document.getElementById("myImg");
  const input = document.getElementById("myInput");

  img.src = "img.jpg";               // 设置图片路径
  input.value = "新值";              // 设置输入框的内容
</script>
```


## 四、修改样式和类名
#### 1️. 直接设置样式（通过 `style`）
```html
<div id="box">内容</div>
<script>
  const element = document.getElementById("box");

  element.style.color = "red";
  element.style.fontSize = "20px";
</script>
```
`style` 属性直接设置的是**内联样式**，优先级高。
#### 2️. 操作类名（通过 `classList`）
```html
<div id="box" class="hidden">文字</div>
<script>
  const element = document.getElementById("box");

  element.classList.add("active");       // 添加类名
  element.classList.remove("hidden");    // 移除类名
  element.classList.toggle("dark-mode"); // 切换类名（有就删，没有就加）
</script>
```
推荐用 `classList` 来管理样式类名，避免写太多内联样式。

## 五、添加、插入、删除元素
#### 1️.创建元素
```js
<script>
    // 创建一个新的 div 元素
    const newDiv = document.createElement("div");

    // 给这个 div 设置文字内容
    newDiv.textContent = "我是新建的 div 元素";

    // 再添加到页面中
    document.body.appendChild(newDiv);
  </script>
```
`appendChild()` 是 JavaScript 提供的一个 DOM 方法，用来把一个元素节点或其他节点添加到某个父元素的最后（作为最后一个子节点）。

#### 2.插入到某个元素前面
```html
<div id="parent">
  <p id="target">我是原有的</p>
</div>
<script>
  const parent = document.getElementById("parent");
  const targetElement = document.getElementById("target");

  const newDiv = document.createElement("div");
  newDiv.textContent = "我是新插入的";

  parent.insertBefore(newDiv, targetElement); // 插入到 targetElement 前面
</script>
```
`insertBefore()` 是 JavaScript 提供的 DOM 方法，用来在某个父节点中，把新节点插入到指定子节点之前。

#### 3. 删除元素
**方式 1：使用 `.remove()`（推荐，现代浏览器支持）**
```html
<body>
  <ul>
    <li id="item1">第 1 项</li>
    <li>第 2 项</li>
  </ul>

  <script>
    const item = document.getElementById("item1");
    item.remove(); // 删除这个 li 元素
  </script>
</body>
```
结果：第 1 项从页面中消失了。

**方式 2：使用 `.removeChild()`（兼容旧浏览器）**
```html
<body>
  <ul id="list">
    <li id="item1">第 1 项</li>
    <li>第 2 项</li>
  </ul>

  <script>
    const list = document.getElementById("list");
    const item = document.getElementById("item1");

    list.removeChild(item); // 从父元素中移除
  </script>
</body>
```
