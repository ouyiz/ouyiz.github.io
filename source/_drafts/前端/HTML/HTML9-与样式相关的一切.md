# 一.样式的作用
样式（CSS）决定网页的**外观和排版**，让你的网页从只有结构（HTML）变得美观、整齐、有吸引力。

# 二. 内联样式
直接在HTML标签的 `style` 属性中写CSS样式。这种方式只会影响该元素，不会影响其他元素。

**示例：**
```html
<p style="color: red; font-size: 20px;">这是一个带有内联样式的段落。</p>
```


# 三. 内部样式

在HTML文档的 `<head>` 标签中，使用 `<style>` 标签来定义样式。样式只作用于当前文档中的元素。

**示例：**
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>内部样式示例</title>
  <style>
    p {
      color: blue;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <p>这是一个带有内部样式的段落。</p>
</body>
</html>
```

# 四. 外部样式
通过 `link` 标签引入外部CSS文件。这是最常用的方法，样式可以在多个HTML文件之间共享，便于统一管理。

**示例：**
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>外部样式示例</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <p>这是一个带有外部样式的段落。</p>
</body>
</html>
```

**外部CSS文件（styles.css）：**
```css
p {
  color: green;
  font-size: 22px;
}
```

