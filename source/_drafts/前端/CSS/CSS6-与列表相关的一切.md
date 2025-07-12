附：

|列表类型|标签|特点|常用场景|
|---|---|---|---|
|无序列表|`<ul>`|项目前有圆点，顺序不重要|导航栏、分类目录|
|有序列表|`<ol>`|项目前有编号，顺序重要|步骤、排名|
|自定义列表|`<dl>`|术语-解释结构|词汇表、问答FAQ|

## 1. `list-style-type` —— 列表项标记类型

**作用**：设置列表项的项目符号样式或编号类型。  
**语法**：
```css
selector {
    list-style-type: value;
}
```
- 常用值包括：
    - `disc`：实心圆（默认值）
    - `circle`：空心圆
    - `square`：实心方块
    - `decimal`：数字编号（1, 2, 3…）
    - `decimal-leading-zero`：带前导零的数字（01, 02…）
    - `lower-alpha`：小写字母编号（a, b, c…）
    - `upper-alpha`：大写字母编号（A, B, C…）
    - `lower-roman`：小写罗马数字（i, ii, iii…）
    - `upper-roman`：大写罗马数字（I, II, III…）
    - `none`：无标记符号

**示例**：
```css
ul {
    list-style-type: square;
}

ol {
    list-style-type: upper-roman;
}
```
示例图：
<img src="file-20250506223351373.png" style="width: 300px; height: auto;">



## 2. `list-style-position` —— 列表项标记位置

**作用**：定义列表项标记是放在内容内部还是外部。  
**语法**：

```css
selector {
    list-style-position: value;
}
```
- `outside`（默认值）：标记符号放在列表项框外部。
- `inside`：标记符号和内容在同一缩进内，可能会出现换行对不齐。

**示例**：
```css
ul {
    list-style-position: inside;
}
```
示例图：
<img src="file-20250506224217208.png" style="width: 500px; height: auto;">


## 3. `list-style-image` —— 自定义列表项标记图像

**作用**：使用图像替换列表项的默认标记。  
**语法**：

```css
selector {
    list-style-image: url("图片路径");
}
```
- 若图像加载失败，浏览器使用 `list-style-type` 指定的备用样式。
- 设置为 `none` 可禁用图像标记。

**示例**：
```css
ul {
    list-style-image: url("star.png");
}
```
示例图：
<img src="file-20250507120021393.png" style="width: 300px; height: auto;">

## 4. `list-style` —— 列表样式简写

**作用**：一次性设置 `list-style-type`、`list-style-position` 和 `list-style-image`。
**语法**：
```css
selector {
    list-style: type position image;
}
```
- 各部分顺序无要求，但通常建议顺序为 `type position image`。
- 也可以单独设置其中一项，其它使用默认值。
**示例**：
```css
ul {
    list-style: square inside url("icon.png");
}
```

## 18.本节代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Background Repeat and Position Demo</title>
    <style></style>
</head>

<body>
    <h2>1. list-style-type —— 列表项标记类型</h2>
    <div>
        <p>无序列表实心方块</p>
        <ul style="list-style-type: square;">
            <li>Item 1</li>
            <li>Item 2</li>
            <li>Item 3</li>
        </ul>
        <p>无序列表空心圆</p>
        <ul style="list-style-type: circle;">
            <li>Item 1</li>
            <li>Item 2</li>
            <li>Item 3</li>
        </ul>
        <p>有序列表小写字母编号</p>
        <ol style="list-style-type: lower-alpha;">
            <li>Item 1</li>
            <li>Item 2</li>
            <li>Item 3</li>
        </ol>
        <p>有序列表小写罗马编号</p>
        <ol style="list-style-type: lower-roman;">
            <li>Item 1</li>
            <li>Item 2</li>
            <li>Item 3</li>
        </ol>
    </div>

    <h2>2. list-style-position —— 列表项标记位置</h2>
    <div>
        <p>outside示例</p>
        <ul style="list-style-position: outside;">
            <li>这是一个非常长的列表项，用于展示标记符号在 outside 时的效果，文字会与列表符号对齐。文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字</li>
            <li>这是一个非常长的列表项，用于展示标记符号在 outside 时的效果，文字会与列表符号对齐。</li>
        </ul>
        <p>inside示例</p>
        <ul style="list-style-position: inside;">
            <li>这是一个非常长的列表项，用于展示标记符号在 inside 时的效果，文字会与标记符号一起缩进。文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字</li>
            <li>这是一个非常长的列表项，用于展示标记符号在 inside 时的效果，文字会与标记符号一起缩进。</li>
        </ul>
    </div>

    <h2>3.  list-style-image —— 自定义列表项标记图像</h2>
    <div>
        <p>自定义列表项标记图像示例</p>
        <ul style="list-style-image: url('images/test.jpg');">
            <li>Item 1</li>
            <li>Item 1</li>
        </ul>
    </div>
</body>

</html>
```
<img src="file-20250507120334698.png" style="width: 500px; height: auto;">
