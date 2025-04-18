# 一.本节代码
```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>HTML 图片标签练习</title>
</head>
<body>

    <h1>HTML 图片标签综合示例</h1>

    <!-- 基本图片展示 -->
    <h2>1️⃣ 最基础的图片</h2>
    <img src="images/example.jpg" alt="风景照片">
  
    <!-- 设置图片尺寸 -->
    <h2>2️⃣ 设置图片大小</h2>
    <img src="images/example.jpg" alt="调整大小的图片" width="300" height="200">

    <!-- 自适应图片 -->
    <h2>3️⃣ 自适应图片（推荐：响应式设计）</h2>
    <img src="images/example.jpg" alt="自适应风景图" style="max-width:100%; height:auto;">

    <!-- 图片加载失败显示alt -->
    <h2>4️⃣ 图片加载失败，显示替代文字</h2>
    <img src="https://www.runoob.com/images/pulit.jpg" alt="图片未找到，替代文字">

    <!-- 懒加载图片 -->
    <h2>5️⃣ 懒加载图片</h2>
    <p>向下滚动后，图片才会加载：</p>
    <img src="https://www.runoob.com/images/pulpit.jpg" alt="大图示例" loading="lazy" width="600">

    <!-- 图片作为链接 -->
    <h2>6️⃣ 点击图片跳转链接</h2>
    <a href="https://www.runoob.com" target="_blank">
        <img src="https://www.runoob.com/images/pulpit.jpg" alt="跳转到菜鸟教程" width="150">
    </a>

    <!-- 图片格式示例（描述） -->
    <h2>7️⃣ 常用图片格式说明</h2>
    <ul>
        <li>JPG：适合风景、人物等照片类图片，压缩小，加载快。</li>
        <li>PNG：适合图标、Logo，支持透明，质量高。</li>
        <li>GIF：适合小型动画，支持动图。</li>
        <li>SVG：矢量图标，网页缩放不失真。</li>
        <li>WEBP：现代格式，兼具高压缩+高质量。</li>
    </ul>

</body>

</html>
```

# 二.基本图片标签：`<img>`

HTML中插入图片，主要用 `<img>` 标签。

**语法：**
```html
<img src="图片路径" alt="替代文字">
```
- `src`（source）：图片文件路径（可以是本地地址或网络地址）。
- `alt`（alternative）：图片无法加载时显示的替代文字，利于用户体验。


# 三.图片常用属性

| 属性名       | 作用           | 示例                            |
| --------- | ------------ | ----------------------------- |
| `src`     | 图片文件路径       | `src="image.jpg"`             |
| `alt`     | 图片加载失败时显示的文本 | `alt="描述信息"`                  |
| `width`   | 图片宽度（像素/百分比） | `width="200"` 或 `width="50%"` |
| `height`  | 图片高度（像素/百分比） | `height="100"`                |
| `title`   | 鼠标悬停时提示文本    | `title="这是图片"`                |
| `loading` | 图片加载时机（懒加载等） | `loading="lazy"`              |
| `style`   | 内联CSS，设置图片样式 | `style="border:1px solid"`    |

# 三.图片路径的写法

| 类型   | 示例                              | 说明            |
| ---- | ------------------------------- | ------------- |
| 绝对路径 | `src="https://abc.com/pic.jpg"` | 从完整网站地址加载     |
| 相对路径 | `src="images/photo.jpg"`        | 从当前页面所在目录寻找文件 |
| 上级目录 | `src="../img/photo.jpg"`        | 返回上级文件夹寻找图片   |

# 四.图片的自适应显示

如果想让图片**根据屏幕大小自动缩放**，可以配合 CSS 设置：

```html
<img src="img.jpg" alt="描述" style="max-width:100%; height:auto;">
```
这样图片不会超过父元素的宽度，适应不同设备，尤其适合移动端。

# 五.图片链接（点击图片跳转）

图片可以包裹在`<a>`标签里，点击图片跳转：

```html
<a href="https://example.com">
    <img src="logo.png" alt="网站LOGO">
</a>
```


# 六.图片的替代文字（alt）

- 图片无法加载时，`alt` 的文本会显示在图片原本的位置；
- 对搜索引擎友好，帮助SEO优化；
- 对视障人士友好，屏幕阅读器会读出 `alt`。
```html
<img src="图片路径" alt="替代文字">
```


# 七.图片的懒加载（Lazy Loading）

现代浏览器支持 `loading="lazy"`，图片只有在**滚动到可视区域时才加载**，加快网页速度：

```html
<img src="big-photo.jpg" alt="风景图" loading="lazy">
```


# 八.图片格式介绍

|格式|适用场景|特点|
|---|---|---|
|`.jpg` / `.jpeg`|照片、风景等色彩图像|有损压缩，体积小|
|`.png`|透明背景、图标、LOGO|支持透明，清晰但体积稍大|
|`.gif`|简单动画或图标|支持动图，色彩少|
|`.svg`|矢量图标，响应式LOGO|可缩放，清晰不失真|
|`.webp`|现代网页图片|高压缩比，质量优秀|


# 九.图片与布局相关的注意点
- `<img>` 是**行内元素**，默认会与文字在一行；


# 十.练习
## 练习1：插入一张图片
请写出代码，在网页中插入一张图片，图片路径为 `images/cat.jpg`，并设置替代文字为“可爱的小猫”。

## 练习2：设置图片大小
在练习1的基础上，设置图片宽度为300像素，高度为自动（保持原比例）。

## 练习3：添加提示信息
请补全代码，使鼠标悬停在图片上时显示文字“这是我的猫”。
```html
<img src="images/cat.jpg" alt="可爱的小猫" _____="这是我的猫">
```

## 练习4：懒加载图片
请写一段代码，插入图片 `img/photo.jpg`，并设置**滚动到视野时才加载**。

## 练习5：图片点击跳转
请写出代码，使图片点击后跳转到网站 `https://www.example.com`，图片路径为 `logo.png`。

## 练习6：自适应图片布局
写出代码，让图片宽度最多不超过父容器，并根据设备自动缩放，路径为 `banner.jpg`，替代文字为“网站横幅”。

## 答案
``` html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 图片标签示例</title>
</head>
<body>

    <h1>HTML 图片标签示例</h1>

    <!-- 插入一张图片 -->
    <h2>练习1：插入一张图片</h2>
    <img src="images/cat.jpg" alt="可爱的小猫">

    <!-- 设置图片大小 -->
    <h2>练习2：设置图片大小</h2>
    <img src="images/cat.jpg" alt="可爱的小猫" width="300" height="auto">

    <!-- 添加图片提示信息 -->
    <h2>练习3：添加提示信息</h2>
    <img src="images/cat.jpg" alt="可爱的小猫" title="这是我的猫">

    <!-- 懒加载图片 -->
    <h2>练习4：懒加载图片</h2>
    <img src="img/photo.jpg" alt="风景图" loading="lazy">

    <!-- 图片点击跳转 -->
    <h2>练习5：图片点击跳转</h2>
    <a href="https://www.example.com">
        <img src="logo.png" alt="网站LOGO">
    </a>

    <!-- 自适应图片布局 -->
    <h2>练习6：自适应图片布局</h2>
    <img src="banner.jpg" alt="网站横幅" style="max-width:100%; height:auto;">


</body>
</html>

```