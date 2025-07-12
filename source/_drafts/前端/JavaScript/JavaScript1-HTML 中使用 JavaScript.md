## 一. 直接在 HTML 标签中写 JavaScript — 内联脚本
直接在 HTML 文件的 `<script>` 标签里写 JavaScript 代码。
```html
<!DOCTYPE html>
<html>
<head>
  <title>JS 内联示例</title>
</head>
<body>
  <h1>欢迎</h1>
  
  <script>
    // 这里写JS代码
    console.log("页面加载完成");
    alert("欢迎访问本站");
  </script>
  
</body>
</html>
```
## 二. 在事件属性中写 JavaScript — 内联事件
直接在 HTML 标签的事件属性里写 JavaScript 代码，比如点击按钮时执行。
```html
<button onclick="alert('你点击了按钮')">点我</button>
```

## 三. 外部引用 JavaScript 文件
把 JavaScript 代码放在单独的 `.js` 文件中，然后用 `<script>` 标签的 `src` 属性引入。
```html
<!DOCTYPE html>
<html>
<head>
  <title>外部JS示例</title>
</head>
<body>
  <h1>欢迎</h1>
  <script src="main.js"></script>
</body>
</html>
```
`main.js` 内容举例：
```js
console.log("这是外部JS文件");
alert("欢迎使用外部JS");
```


## 四. `<script>` 标签的位置
- **放在 `<head>` 里**  
    脚本会在 HTML 内容加载前执行，可能会阻塞页面渲染，除非加上 `defer` 或 `async` 属性。适合一些页面加载前必须运行的初始化代码。
    ```html
    <head>
      <script>
        // 提前运行的初始化代码
      </script>
    </head>
    ```
- **放在 `<body>` 中间**  
    脚本会在解析到它的位置时立即执行。适合希望某段HTML加载后立即运行JS逻辑的场景，但可能导致某些还未加载的元素访问失败。
    ```html
    <body>
      <h1>欢迎</h1>
      <script>
        // 此时<h1>已被解析，可以访问
        document.querySelector('h1').style.color = 'blue';
      </script>
    </body>
    ```
- **放在 `<body>` 底部**（推荐）  
    脚本会在所有HTML元素加载后执行，不会阻塞页面结构的加载，是最常见也最推荐的写法。
    ```html
    <body>
      <h1>页面内容</h1>
      <script>
        // 页面内容加载后运行的JS
      </script>
    </body>
    ```
- **结合 `defer` 属性使用 `<script src="...">`**  
    如果你需要在 `<head>` 中引用外部 JS 文件，但又希望它等页面结构加载后再执行，可以加上 `defer`：
    ```html
    <head>
      <script src="main.js" defer></script>
    </head>
    ```

## 五. `defer` 和 `async` 属性
- `<script src="xxx.js" defer></script>`
	- 浏览器先加载完整个 HTML 页面，- 再执行这些脚本（顺序和写的顺序一致）
- `<script src="xxx.js" async></script>`
	- 脚本“边下载边执行”，加载好了就立即执行，不等HTML加载，也不管顺序
