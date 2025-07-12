## 一.`overflow` 属性的所有取值

|属性值|含义|
|---|---|
|`visible`|默认值，内容不会被裁剪，超出容器会显示出来。|
|`hidden`|超出部分内容被裁剪，不会显示，也不能滚动。|
|`scroll`|不论是否溢出，总是显示滚动条，用户可滚动查看内容。|
|`auto`|当内容溢出时才显示滚动条，不溢出则无滚动条。|
|`clip`|内容被裁剪，不能滚动，是 `hidden` 的轻量版本。|

## 二.`overflow` 基本用法
- 控制内容在溢出元素盒子时的表现。
- 只有当元素有**固定高度或宽度**时才会生效。

```css
div {
  width: 200px;
  height: 100px;
  overflow: auto;
}
```

## 三.`overflow-x` 和 `overflow-y`（方向控制）

|属性|含义说明|
|---|---|
|`overflow-x`|控制水平方向上的内容溢出行为|
|`overflow-y`|控制垂直方向上的内容溢出行为|

```css
div {
  overflow-x: scroll;
  overflow-y: hidden;
}
```


## 四.`overflow: visible`（默认值）
- 不裁剪内容，内容会继续延伸出元素外部。
- 不出现滚动条。
- 容易导致布局混乱。
```css
div {
  overflow: visible;
}
```

## 五.`overflow: hidden`
- 裁剪掉多余内容。
- 不出现滚动条，也无法滚动查看被隐藏的内容。
	- `hidden` 实际上还是创建了一个“可滚动容器”，只是禁止了滚动
- 常用于隐藏超出部分，如背景裁剪、清除浮动等。
```css
div {
  overflow: hidden;
}
```

## 六.`overflow: scroll`
- 内容无论是否溢出都会出现滚动条。
- 会始终保留滚动条空间（可能是灰的）。
- 不推荐用于美观设计，但对可访问性有帮助。

```css
div {
  overflow: scroll;
}
```

## 七.`overflow: auto`
- 当内容溢出容器时，自动出现滚动条。
- 内容没溢出就不显示滚动条，体验较好。
```css
div {
  overflow: auto;
}
```

## 八.`overflow: clip`
- 内容被裁剪。
- 与 `hidden` 类似，但不创建滚动容器（性能更好）。
- 常用于裁剪遮罩等新式布局中。
```css
div {
  overflow: clip;
}
```


## 九.相关扩展属性：`text-overflow`
- 用于文本溢出时显示省略号（需搭配 `overflow` 和 `white-space`）。
```css
.ellipsis {
	overflow: hidden;          // 把溢出的部分隐藏
	white-space: nowrap;       // 不换行（整行显示）
	text-overflow: ellipsis;   // 把溢出的文本用省略号 "..." 表示
}
```

## 十.相关扩展属性：`scroll-behavior`
- 用来控制页面或容器滚动时的动画效果
-  `auto`（默认）：瞬间跳过去，没有动画
- `smooth`：平滑滚动，带动画效果
```css
html {
  scroll-behavior: smooth;
}
```


## 十一.本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>CSS Overflow 示例</title>
  <style>
    html {
      scroll-behavior: smooth; /* 十.scroll-behavior */
    }

    body {
      font-family: sans-serif;
      padding: 20px;
    }

    .box {
      width: 200px;
      height: 100px;
      margin: 20px;
      border: 2px solid #333;
      padding: 10px;
      background-color: #f9f9f9;
    }

    .visible-box {
      overflow: visible;
    }

    .hidden-box {
      overflow: hidden;
    }

    .scroll-box {
      overflow: scroll;
    }

    .auto-box {
      overflow: auto;
    }

    .clip-box {
      overflow: clip;
    }

    .direction-box {
      overflow-y: hidden;
      overflow-x: scroll;
      white-space: nowrap;
    }

    .ellipsis {
      width: 200px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      border: 1px solid #aaa;
      margin: 10px 0;
    }

    .nav {
      position: fixed;
      top: 10px;
      right: 10px;
      background: #eee;
      padding: 10px;
      border: 1px solid #ccc;
    }

    section {
      margin-top: 80px;
    }
  </style>
</head>
<body>

  <div class="nav">
    <a href="#clip">跳转到 Clip</a>
  </div>

  <h1>CSS Overflow 示例</h1>

  <section>
    <h2>1. overflow: visible (默认)</h2>
    <div class="box visible-box">
      这个盒子的内容很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长。
    </div>
  </section>

  <section>
    <h2>2. overflow: hidden</h2>
    <div class="box hidden-box">
      这个内容会被裁剪，不可滚动，不可见。这个内容会被裁剪，不可滚动，不可见。很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    </div>
  </section>

  <section>
    <h2>3. overflow: scroll</h2>
    <div class="box scroll-box">
      这个内容可以滚动，即使不溢出也有滚动条。这个内容可以滚动，即使不溢出也有滚动条。
    </div>
  </section>

  <section>
    <h2>4. overflow: auto</h2>
    <div class="box auto-box">
      只有在内容溢出时才出现滚动条。只有在内容溢出时才出现滚动条。只有在内容溢出时才出现滚动条。很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    </div>
  </section>

  <section id="clip">
    <h2>5. overflow: clip</h2>
    <div class="box clip-box">
      内容被裁剪，不会出现滚动条。性能比 hidden 更高。内容被裁剪，不会出现滚动条。很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    </div>
  </section>

  <section>
    <h2>6. overflow-x / overflow-y</h2>
    <div class="box direction-box">
      水平可以滚动，但垂直方向被裁剪。水平方向很长很长很长很长很长很长很长很长很长很长。很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    </div>
  </section>

  <section>
    <h2>7. text-overflow: ellipsis</h2>
    <div class="ellipsis">
      这是一段非常非常非常非常非常非常非常长的文字，它将被省略号显示。
    </div>
  </section>

</body>
</html>

```