## 一.`position` 属性的所有取值

| 属性值        | 含义                                           |
| ---------- | -------------------------------------------- |
| `static`   | 默认值，按正常文档流排列，不可使用 `top/right/bottom/left`。   |
| `relative` | 相对定位，相对于元素自身原来的位置进行偏移。                       |
| `absolute` | 绝对定位，相对于**最近的定位祖先元素**（非 `static`）定位。         |
| `fixed`    | 固定定位，相对于浏览器视口（窗口）进行定位，通常用于固定头部、侧边栏等。         |
| `sticky`   | 粘性定位，在一定滚动区域内表现为 `relative`，超出区域则变成 `fixed`。 |


## 二. `static`（默认值）
- 所有元素默认是 `static`。
- 不受 `top` / `left` / `right` / `bottom` 等影响。
- 不会脱离文档流（`static` 元素就是乖乖地排队，不会飞来飞去，也不会重叠在其他元素上）
```css
div {
  position: static;
}
```


> 什么是文档流
> 就是元素一个接一个地往下排列
> `[标题]`
   `[段落1]`
   `[段落2]`
   `[图片]`


## 三. `relative`（相对定位）
- 相对于元素原始位置进行偏移(从原来应该出现的位置出发，偏移一点点)
- 保留原来的空间（不会影响周围元素布局）。
```css
div {
  position: relative;
  top: 10px;
  left: 20px;
}
```


## 四. `absolute`（绝对定位）
- 相对于最近的非static定位的祖先元素定位。
	- 网页是一个层层嵌套的盒子结构
	- 如果你设置了 `position: absolute`，它会往上找，看看哪个祖先元素设置了 `position: relative` / `absolute` / `fixed` / `sticky`；
	- 找到这个最近的“定位父亲”之后，它就以这个盒子的左上角为参考来定位！
	- 若无定位祖先元素，则相对于 `<html>`（即页面整体）定位。
- 脱离文档流（不占空间），网页中后面的内容不会考虑它的存在。
```css
div {
  position: absolute;
  top: 0;
  right: 0;
}
```


## 五. `fixed`（固定定位）
- 相对于浏览器视口定位。
- 脱离文档流，不会占位置
- 可以悬浮在页面上，覆盖其他元素。
- 页面滚动时不会移动，常用于悬浮按钮、固定菜单栏等。
```css
div {
  position: fixed;
  bottom: 10px;
  right: 10px;
}
```


## 六. `sticky`（粘性定位）
- 结合 `relative` 和 `fixed` 的效果
	- 一开始，元素和普通元素一样，属于文档流（这是 `relative` 的表现）；
	- 当它快滚出屏幕、触碰到 `top`（或 `left`、`bottom` 等）的位置时，它会粘住这个位置，不再往上滚（这就是 `fixed` 的表现）；
	- 它会一直“钉”在那里，直到它的父元素被滚出视口，它才会随父元素一起消失。

```css
div {
  position: sticky;
  top: 0;
}
```
⚠️ 需要设置 `top` / `left` 才有效，且**父元素不能有 `overflow: hidden` 或 `overflow: auto`**。

## 七.相关属性
**常与 `position` 一起使用的属性：**
- `top`：距离顶部的偏移量。
- `right`：距离右侧的偏移量。
- `bottom`：距离底部的偏移量。
- `left`：距离左侧的偏移量。
- `z-index`：控制元素在 z 轴（前后顺序）上的层级。值越大，越在前面。
```css
div {
  position: absolute;
  top: 20px;
  left: 30px;
  z-index: 999;
}
```

## 八.布局技巧总结

| 需求       | 推荐用法                                  |
| -------- | ------------------------------------- |
| 固定顶部导航栏  | `position: fixed; top: 0;`            |
| 绝对定位子元素  | 父设 `position: relative`，子设 `absolute` |
| 弹窗 / 居中框 | `absolute + transform` 居中             |
| 滚动中吸顶元素  | `position: sticky; top: 0;`           |

## 九.本节代码
```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>Position 属性完整示例</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      height: 2000px; /* 让页面可以滚动 */
      font-family: sans-serif;
    }

    .section {
      padding: 20px;
      border-bottom: 1px solid #ccc;
    }

    .box {
      width: 150px;
      height: 80px;
      line-height: 80px;
      color: white;
      text-align: center;
      margin-bottom: 10px;
    }

    .static-box {
      background-color: gray;
      position: static;
    }

    .relative-box {
      background-color: blue;
      position: relative;
      top: 10px;
      left: 20px;
    }

    .absolute-wrapper {
      position: relative; /* 为了 absolute-box 提供参考点 */
      height: 120px;
      background-color: #eee;
    }

    .absolute-box {
      background-color: red;
      position: absolute;
      top: 10px;
      right: 10px;
    }

    .fixed-box {
      background-color: green;
      position: fixed;
      bottom: 10px;
      right: 10px;
      z-index: 1000;
    }

    .sticky-wrapper {
      height: 600px;
      background-color: #f8f8f8;
      padding: 0 10px;
    }

    .sticky-box {
      background-color: orange;
      position: sticky;
      top: 0;
      z-index: 500;
    }
  </style>
</head>
<body>

  <div class="section">
    <h2>1. Static（默认定位）</h2>
    <div class="box static-box">Static</div>
    <p>这是默认排列方式，位置固定在文档流中，不受 top/left 控制。</p>
  </div>

  <div class="section">
    <h2>2. Relative（相对定位）</h2>
    <div class="box relative-box">Relative</div>
    <p>偏移后保留原始位置，不影响其他元素布局。</p>
  </div>

  <div class="section">
    <h2>3. Absolute（绝对定位）</h2>
    <div class="absolute-wrapper">
      <div class="box absolute-box">Absolute</div>
      <p>红色框是绝对定位，相对于灰色父容器定位。</p>
    </div>
  </div>

  <div class="section">
    <h2>4. Fixed（固定定位）</h2>
    <div class="box fixed-box">Fixed</div>
    <p>绿色框是固定定位，始终在右下角，页面滚动也不动。</p>
  </div>

  <div class="section sticky-wrapper">
    <h2 class="box sticky-box">5. Sticky（粘性定位）</h2>
    <p>橙色标题会在你滚动到顶部时“吸附”在那里，不会被继续滚出视口，直到这段内容滚完。</p>
    <p>多添加几段文字用于滚动效果测试。</p>
    <p>...</p>
    <p>...</p>
    <p>...</p>
    <p>...</p>
  </div>

</body>
</html>
```