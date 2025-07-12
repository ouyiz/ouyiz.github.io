## 一. `window.alert()`
- 作用：弹出一个浏览器的提示对话框。
- 使用场景：调试时临时提示信息，或用户操作反馈
```html
<script>
  alert("你好，欢迎访问本网站！");
</script>
```
**注意**：这个弹窗会阻塞页面的其他操作，直到用户点击“确定”。

## 二. `document.write()`
- 作用：把内容写入 HTML 页面中。
- 使用场景：调试中快速测试输出
```html
<script>
  document.write("<h2>这是使用 document.write() 输出的内容</h2>");
</script>
```
**注意**：如果在页面加载完成后再调用 `document.write()`，会清空整个页面内容。

## 三. `element.innerHTML`
- 作用：修改某个 HTML 元素的内容。
- 使用场景：常用于动态更新网页内容，比如点击按钮后改变某段文字。
```html
<body>
  <p id="demo">原始文字</p>
  <button onclick="document.getElementById('demo').innerHTML = '内容已更新！'">点我改变文字</button>
</body>
```

## 四. `console.log()`
- 作用：不影响页面结构，将信息输出到控制台。
- 使用场景：调试 JavaScript 代码时查看变量值、函数调用过程等。
```html
<script>
  let name = "小明";
  console.log("用户名为：" + name);
</script>
```
打开浏览器 → 按 F12（或右键 → 检查）→ 进入“控制台”（Console）标签即可看到输出。

## 五.本节代码
```html
<!DOCTYPE html>
<html>
<head>
  <title>输出数据演示</title>
</head>
<body>
  <h2 id="title">原始标题</h2>
  <p>element.innerHTML：修改某个 HTML 元素的内容。</p>
  <button onclick="changeTitle()">点击我</button>
  <script>
    function changeTitle() {
      document.getElementById("title").innerHTML = "修改后的标题";
      }
  </script>
  <p>window.alert()：弹出一个浏览器的提示对话框。</p>
  <button onclick="alert('哈哈哈')">点击我</button>
  <p>document.write()：把内容写入 HTML 页面中。</p>
  <button onclick="document.write('哈哈哈')">点击我</button>
  <p>console.log()：在浏览器的控制台输出内容。</p>
  <button onclick="console.log('哈哈哈')">点击我</button>
</body>
</html>

```