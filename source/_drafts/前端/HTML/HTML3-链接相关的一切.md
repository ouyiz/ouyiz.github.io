# 一.本节代码
```html
<!DOCTYPE html>

<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>HTML 链接完整示例</title>
</head>
<body>
  
    <h1>🔗 HTML 链接完整示例</h1>

    <!-- 1. 外部链接 -->
    <p>
        访问外部网站：
        <a href="https://www.bing.com/" target="_blank" title="搜索引擎">Bing</a>
    </p>
  
    <!-- 2. 内部链接（假设about.html在同一目录） -->
    <p>
        访问本站内的页面：
        <a href="html文本.html">文本</a>
    </p>

    <!-- 3. 页面内锚点链接 -->
    <p>
        快速跳转到页面底部：
        <a href="#bottom">点我跳转</a>
    </p>

    <!-- 4. 邮件链接 -->
    <p>
        联系我们：
        <a href="mailto:abc@example.com">发送邮件</a>
    </p>

    <!-- 5. 电话链接（适合移动端） -->
    <p>
        拨打电话：
        <a href="tel:123456789">拨打 123456789</a>
    </p>

    <!-- 6. 下载链接 -->
    <p>
        下载文件：
        <a href="file.zip" download>点击下载 file.zip</a>
    </p>

    <!-- 7. 图片链接 -->
    <p>
        点击图片访问网站：
        <a href="https://www.runoob.com" target="_blank">
            <img src="https://www.runoob.com/images/pulpit.jpg" alt="菜鸟教程 Logo" width="150">
        </a>
    </p>

    <!-- 8. 占位链接 -->
    <p>
        功能开发中：
        <a href="#">敬请期待</a>
    </p>

    <!-- 页面内容填充，方便测试锚点 -->
    <div style="height: 600px;"></div>

    <!-- 9. 页面锚点目标 -->
    <p id="bottom"><strong>🎉 已跳转到页面底部！</strong></p>

</body>

</html>
```


# 二.链接的基本标签：`<a>`

`<a>` 标签用来创建**超链接（hyperlink）**，实现页面跳转，点击后跳到其他网页、网站、文件、图片或邮箱。

**基本语法：**

```html
<a href="目标地址">显示的文本</a>
```



# 三.`href` 属性

`href` 是超链接最重要的属性，表示跳转的目标地址：

| 目标地址类型 | 示例                              | 说明            |
| ------ | ------------------------------- | ------------- |
| 外部链接   | `href="https://runoob.com"`     | 跳转到其他网站       |
| 内部链接   | `href="about.html"`             | 跳转到自己网站的某个页面  |
| 页面锚点   | `href="#section1"`              | 跳转到当前页面的特定位置  |
| 邮件地址链接 | `href="mailto:abc@example.com"` | 点击后自动打开邮件客户端  |
| 电话链接   | `href="tel:123456789"`          | 点击后拨打电话（移动设备） |



# 四. `target` 属性

控制链接打开的位置：

| 属性值         | 作用描述               |
| ----------- | ------------------ |
| `_self`（默认） | 当前窗口打开             |
| `_blank`    | 新窗口或新标签页打开         |
| `_parent`   | 在父框架中打开（iframe相关）  |
| `_top`      | 在最顶层窗口打开（iframe相关） |

**示例：**

```html
<a href="https://runoob.com" target="_blank">在新标签页打开链接</a>
```



# 五. `title` 属性

鼠标悬停时显示提示文字：

```html
<a href="https://runoob.com" title="访问菜鸟教程">点击我</a>
```



# 六.锚点链接（页面内跳转）
锚点链接由两部分组成：
- 锚点目标位置：`id`
	- 先在你想跳转的位置，设置一个标签，添加 `id` 属性
	- `<p id="section1">这里是第1部分</p>`
- 跳转链接：`href="#id"`
	- 在链接处，使用 `href="#锚点名称"` 指向这个位置
	- `<a href="#section1">跳到第1部分</a>`

先在你想跳转的位置，设置一个标签，添加 `id` 属性：
可以用 `id` + `href="#id"` 实现**页面内部跳转**。

```html
<a href="#bottom">跳到底部</a>

<p id="bottom">这里是页面底部</p>
```



# 七.下载链接

`<a>` 可以配合 `download` 属性，实现在点击时直接下载文件：

```html
<a href="file.zip" download>点击下载文件</a>
```



# 八.图片链接

链接不仅可以包裹文字，也可以包裹图片：

```html
<a href="https://www.bing.com">
  <img src="logo.png" alt="点击跳转到网站">
</a>
```



# 九.空链接 & 占位链接

如果链接还没准备好，可以先写 `#` 作为占位：

```html
<a href="#">这是一个占位链接</a>
```


# 十.练习

## 练习 1：基础超链接
请用 `<a>` 标签，写一个能点击跳转到 [https://www.runoob.com](https://www.runoob.com/) 的超链接，显示文字为：**访问菜鸟教程**。

## 练习 2：新标签页打开链接
写一个跳转到 [https://www.bilibili.com](https://www.bilibili.com/) 的链接，要求点击后在**新标签页**打开，显示文字：**B站首页**。

## 练习 3：内部链接
假设你的项目里有一个 `about.html` 页面，写一个跳转到该页面的链接，文字显示：**关于我们**。

## 练习 4：锚点链接（页面内跳转）
页面内设置一个跳转，点击文字：“跳转到底部”，页面自动滚动到最后一个写有 `id="footer"` 的段落。

## 练习 5：邮件链接
写一个点击后能打开默认邮件软件，收件人是：`test@example.com` 的超链接，文字为：**发送邮件**。

## 练习 6：电话链接
写一个点击后能在手机上拨打电话 `123456789` 的超链接，文字为：**联系客服**。

## 练习 7：下载链接
写一个下载 `game.zip` 文件的链接，点击文字为：**下载游戏安装包**。

## 练习 8：图片链接
用一张图片 `logo.png` 做链接，点击后跳转到 `https://www.google.com`。

## 练习 9：占位链接
写一个未完成功能的按钮链接，先用 `#` 占位，文字为：**敬请期待**。

## 答案
``` html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>HTML 链接练习示范</title>
</head>
<body>

    <h1>HTML 链接示范</h1>

    <!-- 1. 外部链接 -->
    <p><a href="https://runoob.com">访问菜鸟教程</a></p>

    <!-- 2. 新标签页打开链接 -->
    <p><a href="https://www.bilibili.com" target="_blank">在新标签页打开B站首页</a></p>

    <!-- 3. 内部链接 -->
    <p><a href="about.html">跳转到网站内部的关于页面</a></p>

    <!-- 4. 页面内跳转（锚点链接） -->
    <p><a href="#bottom">跳转到页面底部</a></p>

    <!-- 占位内容，模拟中间一大段 -->
    <p>这里是一些占位内容，滚动查看锚点效果。</p>
    <p>这里是一些占位内容，滚动查看锚点效果。</p>
    <p>这里是一些占位内容，滚动查看锚点效果。</p>
    <p>这里是一些占位内容，滚动查看锚点效果。</p>

    <!-- 锚点目标位置 -->
    <p id="bottom">锚点目标：这是页面底部</p>

    <!-- 5. 邮件链接 -->
    <p><a href="mailto:test@example.com">发送邮件给我们</a></p>

    <!-- 6. 电话链接 -->
    <p><a href="tel:123456789">拨打客服电话</a></p>

    <!-- 7. 下载链接 -->
    <p><a href="game.zip" download>点击下载游戏安装包</a></p>

    <!-- 8. 图片链接 -->
    <p>
        <a href="https://www.google.com" target="_blank">
            <img src="logo.png" alt="点击跳转到Google" width="100">
        </a>
    </p>

    <!-- 9. 占位链接 -->
    <p><a href="#">这是一个占位链接，暂时无跳转</a></p>

</body>
</html>

```

