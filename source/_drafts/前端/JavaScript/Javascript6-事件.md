
## 一、事件基础概念
**1.什么是事件？**
事件是指浏览器中某个操作发生的“通知”，这个操作可以是用户行为（如点击按钮、按下键盘、移动鼠标），也可以是系统行为（如页面加载完成、网络出错等）。事件发生时，浏览器会创建一个事件对象，并将其传给监听这个事件的处理函数（监听器）。
**2.事件的生命周期**
事件在浏览器中大致会经历以下过程：
```
触发 → 捕获阶段（从外向内） → 目标阶段（事件实际发生的元素） → 冒泡阶段（从内向外）
1. 捕获阶段（由 window → html → body → 目标元素的父级）
2. 目标阶段（事件真正触发的元素）
3. 冒泡阶段（由目标元素 → body → html → window）
```

**3.常见的事件类型（扩展说明）**

|类型|示例事件名|说明|
|---|---|---|
|鼠标事件|`click`, `dblclick`, `mousedown`, `mouseup`, `mouseover`, `mouseout`, `mousemove`, `contextmenu`|和鼠标动作有关的事件，例如点击、右键、悬停等|
|键盘事件|`keydown`, `keypress`, `keyup`|和键盘输入有关，`keypress` 已不推荐使用|
|表单事件|`submit`, `change`, `input`, `focus`, `blur`, `reset`|表单控件交互，如输入、提交、焦点切换等|
|文档/窗口事件|`load`, `resize`, `scroll`, `unload`, `DOMContentLoaded`|页面加载/改变大小/滚动时触发|
|拖放事件|`dragstart`, `drag`, `dragover`, `drop`, `dragend`|拖动元素和放置相关的事件|
|媒体事件|`play`, `pause`, `ended`, `volumechange`|视频、音频控制相关|
|触摸事件|`touchstart`, `touchmove`, `touchend`, `touchcancel`|移动端专用，处理手指的触摸操作|
|焦点事件|`focus`, `blur`, `focusin`, `focusout`|控件获得/失去焦点|
|剪贴板事件|`copy`, `cut`, `paste`|和复制、剪切、粘贴操作相关|
|自定义事件|自定义事件名（如 `login-success`）|程序员可以用 `new CustomEvent()` 创建自定义事件|

## 二、事件绑定（监听）

我们使用**事件监听器**来告诉浏览器：**“当某个事件发生时，请调用这个函数来处理它。”**
**1.第一种方式：HTML 内联绑定（不推荐）**
```html
<button onclick="alert('Clicked')">Click me</button>
```

**2.第二种方式：DOM 0 级事件（element.onclick）**

```js
 <button id="btn">Click Me</button>
  <script>
    const btn = document.getElementById("btn");
    btn.onclick = function() {
      alert("Button clicked");
    };
  </script>
```
- 使用 JavaScript 为元素设置一个属性（`onclick`）
- 缺点：同一事件只能绑定一个函数，后绑定会覆盖前面绑定的函数
**3.第三种方式：DOM 2 级事件绑定（`addEventListener()`）**
```js
<button id="btn">Click Me</button>
  <script>
    const btn = document.getElementById("btn");

    btn.addEventListener("click", function () {
      console.log("Clicked!");
    });
  </script>
```
- 同一个事件可以绑定多个处理函数
- 可以控制事件在捕获阶段或冒泡阶段触发
- 支持更多参数选项（如 once、passive）
ES6 后 `addEventListener` 支持传递对象作为第三个参数，替代布尔值：
```js
btn.addEventListener("click", () => {
  console.log("clicked only once");
}, { once: true }); // 绑定后自动移除监听器
```
参数说明：

|属性|说明|
|---|---|
|`capture`|是否在捕获阶段触发事件（默认 false）|
|`once`|是否只触发一次|
|`passive`|表示不会调用 `preventDefault()`，优化性能，常用于 scroll、touch 事件|


**5.移除事件监听器**
- 只有使用**命名函数**绑定事件时，才能移除它
```js
function handleClick() {
  console.log("Clicked");
}

btn.addEventListener("click", handleClick);
// ...
btn.removeEventListener("click", handleClick);
```

## 三、事件对象（`event` 对象）

当事件触发时，事件处理函数会自动接收一个事件对象作为参数，它是 `Event` 类型的实例，包含了本次事件的全部信息。

**基本结构：**
```js
element.addEventListener("click", function (event) {
  console.log(event); // 输出事件对象
});
```


**常用属性和方法：**

|属性/方法|含义|
|---|---|
|`type`|事件的类型（如 `"click"`）|
|`target`|事件的触发源（真正触发事件的元素）|
|`currentTarget`|当前绑定事件的元素（和 `this` 类似）|
|`timeStamp`|事件触发的时间戳（单位：毫秒）|
|`eventPhase`|当前事件处于哪个阶段（1=捕获，2=目标，3=冒泡）|
|`bubbles`|是否会冒泡|
|`cancelable`|是否可以取消事件默认行为|
|`defaultPrevented`|是否已经被 `preventDefault()` 调用|
|`preventDefault()`|阻止事件的默认行为（如表单提交、链接跳转）|
|`stopPropagation()`|阻止事件继续传播（阻止冒泡或捕获）|
|`stopImmediatePropagation()`|阻止事件传播并阻止其他监听器触发（同类型事件）|

**鼠标事件专有属性：**

|属性|说明|
|---|---|
|`clientX/Y`|鼠标相对于浏览器视口的位置|
|`pageX/Y`|鼠标相对于文档的位置|
|`screenX/Y`|鼠标相对于屏幕的位置|
|`button`|哪个鼠标键被点击（0 左键，1 中键，2 右键）|
|`ctrlKey`, `shiftKey`, `altKey`, `metaKey`|是否按下了这些修饰键|

**示例：阻止默认行为 + 停止传播**
```js
document.getElementById("link").addEventListener("click", function (e) {
  e.preventDefault();        // 阻止跳转
  e.stopPropagation();       // 阻止冒泡
  console.log("链接点击被拦截");
});
```
