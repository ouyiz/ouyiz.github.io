附：
HTML使用 `<table>` 标签创建表格，常见结构如下：

|标签|作用|
|---|---|
|`<table>`|创建表格|
|`<tr>`|表格的行（table row）|
|`<td>`|表格的单元格（table data）|
|`<th>`|表头单元格（table header）|

## 1. `border-collapse` —— 表格边框合并方式

**作用**：设置是否将表格的单元格边框合并为单一边框。  
**语法**：

```css
table {
    border-collapse: value;
}
```
- 常用值包括：
    - `separate`：分离边框（默认值）
    - `collapse`：合并边框

**示例**：
```css
table {
    border-collapse: collapse;
    border: 1px solid black;
}
```

示例图：  
<img src="file-20250507122150168.png" style="width: 300px; height: auto;">

## 2. `border-spacing` —— 表格单元格之间的间距

**作用**：在边框未合并时设置单元格之间的间距。  
**语法**：
```css
table {
    border-spacing: horizontal vertical;
}
```
- 单位通常为 `px` 或 `em`
- 若只写一个值，水平和垂直间距相同

**示例**：
```css
table {
    border-spacing: 10px 20px;
}
```
示例图：  
<img src="file-20250509200551765.png" style="width: 300px; height: auto;">

## 3. `table-layout` —— 表格布局算法

**作用**：设置表格的列宽计算方式。也就是表格的列宽会不会自动调节  
**语法**：
```css
table {
    table-layout: value;
}
```
- 常用值包括：
    - `auto`：自动布局（默认），表格会根据“内容”的长度，自动调整列宽
    - `fixed`：固定布局（列宽不受内容影响，依据第一行或 `width` 设置）

**示例**：
```css
table {
    table-layout: fixed;
    width: 100%;
}
```
示例图：
<img src="file-20250509201308318.png" style="width: 300px; height: auto;">

## 4. `caption-side` —— 表格标题的位置
**作用**：设置 `<caption>` 元素在表格中的位置（`<caption>` 是 HTML 中专门用来给表格加标题 的标签）
**语法**：
```css
table {
    caption-side: value;
}
```
- 常用值包括：
    - `top`：在表格上方（默认）
    - `bottom`：在表格下方

**示例**：
```css
table {
    caption-side: bottom;
}
```
示例图：
<img src="file-20250509202034484.png" style="width: 300px; height: auto;">


## 5. `empty-cells` —— 是否显示空单元格的边框

**作用**：控制是否为无内容的单元格绘制边框。  
**语法**：
```css
table {
    empty-cells: value;
}
```
- 常用值包括：
    - `show`：显示空单元格边框（默认）
    - `hide`：隐藏空单元格边框

**示例**：

```css
table {
    empty-cells: hide;
}
```
<img src="file-20250509202727145.png" style="width: 300px; height: auto;">


## 6. `vertical-align` —— 表格单元格内容的垂直对齐方式

**作用**：设置表格单元格中内容的垂直对齐方式。  
**语法**：

```css
td, th {
    vertical-align: value;
}
```
- 常用值包括
    - `top`：顶端对齐
    - `middle`：垂直居中（默认）
    - `bottom`：底部对齐
**示例**：
```css
td {
    vertical-align: top;
}
```
示例图：
<img src="file-20250509203813715.png" style="width: 300px; height: auto;">

## 7. `text-align` —— 表格文字的水平对齐方式

**作用**：设置表格单元格中文本的水平对齐方式。  
**语法**：
```css
td, th {
    text-align: value;
}
```
- 常用值包括：
    - `left`：左对齐
    - `center`：居中对齐
    - `right`：右对齐
**示例**：
```css
th {
    text-align: center;
}
```
示例图：
<img src="file-20250509204134139.png" style="width: 300px; height: auto;">

## 8. `width` 和 `height` —— 单元格或表格尺寸
**作用**：设置表格或单元格的宽度和高度。  
**语法**：
```css
table, th, td {
    width: value;
    height: value;
}
```

**示例**：
```css
td {
    width: 100px;
    height: 50px;
}
```
<img src="file-20250509204637140.png" style="width: 300px; height: auto;">

## 9. `background-color` —— 单元格背景色
**作用**：设置表格单元格的背景颜色。  
**语法**：
```css
td, th {
    background-color: color;
}
```

**示例**：
```css
th {
    background-color: lightgray;
}
```
示例图：
<img src="file-20250509204954568.png" style="width: 300px; height: auto;">

## 10. `border` —— 表格边框样式
**作用**：设置表格或单元格的边框线。  
**语法**：
```css
table, th, td {
    border: width style color;
}
```

**示例**：
```css
table, th, td {
    border: 1px solid black;
}
```
示例图：
<img src="file-20250509205244880.png" style="width: 300px; height: auto;">


## 11.本节代码
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
    <h2>1. border-collapse —— 表格边框合并方式</h2>
    <div>
        <p>collapse合并边框示例</p>
        <table border="1" style="border-collapse: collapse;">
            <tr>
                <th>head1</th>
                <th>head2</th>
                <th>head3</th>
            <tr>
            <tr>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <td>4</td>
                <td>5</td>
                <td>6</td>
            </tr>
        </table>
        <p>separate不合并边框示例</p>
        <table  border="1" style="border-collapse: separate;">
            <tr>
                <th>head1</th>
                <th>head2</th>
                <th>head3</th>
            <tr>
            <tr>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <td>4</td>
                <td>5</td>
                <td>6</td>
            </tr>
        </table>
    </div>

    <h2>2. border-spacing —— 表格单元格之间的间距</h2>
    <div>
        <table  border="1" style="border-collapse: separate;border-spacing: 50px 10px;">
            <tr>
                <th>head1</th>
                <th>head2</th>
                <th>head3</th>
            <tr>
            <tr>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <td>4</td>
                <td>5</td>
                <td>6</td>
            </tr>
        </table>
    </div>

    <h2>3. table-layout —— 表格布局算法</h2>
    <div>
        <p>auto自动算法示例</p>
        <table border="1" style="border-collapse: separate;table-layout: auto;">
            <tr>
                <th>名字</th>
                <th>介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td>我是小2，一个小2，两个小2</td>
            </tr>
        </table>
        <p>fixed固定算法示例</p>
        <table border="1" style="border-collapse: separate;table-layout: fixed;width: 200px;">
            <tr>
                <th>名字</th>
                <th>介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td>我是小2，一个小2，两个小2</td>
            </tr>
        </table>
    </div>

    <h2>4. caption-side —— 表格标题的位置</h2>
    <div>
        <table border="1" style="border-collapse: collapse;width: 300px;caption-side: top;">
            <caption>人物介绍表</caption>
            <tr>
                <th>名字</th>
                <th>介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td>我是小2，一个小2，两个小2</td>
            </tr>
          </table>
    </div>

    <h2>5. empty-cells —— 是否显示空单元格的边框</h2>
    <div>
        <p>empty-cells: show 显示空单元格的边框</p>
        <table border="1" style="border-collapse: separate;empty-cells: show;">
            <tr>
                <th>名字</th>
                <th>介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td></td>
            </tr>
        </table>
        <p>empty-cells: hide; 隐藏空单元格的边框</p>
        <table border="1" style="border-collapse: separate;empty-cells: hide;">
            <tr>
                <th>名字</th>
                <th>介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td></td>
            </tr>
        </table>
    </div>

    <h2>6. vertical-align —— 表格单元格垂直对齐方式</h2>
    <div>
        <table border="1" style="border-collapse: collapse;height: 150px;">
            <caption>人物介绍表</caption>
            <tr>
                <th style="vertical-align: top;">名字</th>
                <th style="vertical-align: top;">介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td>我是小2，一个小2，两个小2</td>
            </tr>
        </table>
    </div>

    <h2>7. text-align —— 表格文字的水平对齐方式</h2>
    <div>
        <table border="1" style="border-collapse: collapse;height: 100px;">
            <caption>人物介绍表</caption>
            <tr>
                <th style="text-align: left;">名字</th>
                <th style="text-align: left;">介绍</th>
            <tr>
            <tr>
                <td>小1</td>
                <td>我是小1</td>
            </tr>
            <tr>
                <td>小2</td>
                <td>我是小2，一个小2，两个小2</td>
            </tr>
        </table>
    </div>

    <h2> 8. width和 height —— 单元格或表格尺寸</h2>
    <table border="1" style="border-collapse: separate;">
        <caption>人物介绍表</caption>
        <tr>
            <th style="height: 50px;">高为50px</th>
            <th style="height: 20px;">高为20px</th>
        <tr>
        <tr>
            <td style="height: 50px;">高为50px</td>
            <td style="height: 30px;">高为30px</td>
        </tr>
        <tr>
            <td >结论</td>
            <td>同一行高度不同，选了大的</td>
        </tr>
    </table>

    <h2>9. background-color —— 单元格背景色</h2>
    <table border="1" style="border-collapse: separate;">
        <tr>
            <th style="background-color: bisque;">名字</th>
            <th style="background-color: greenyellow;">介绍</th>
        <tr>
        <tr>
            <td style="background-color: yellowgreen;">小1</td>
            <td style="background-color: burlywood;">我是小1</td>
        </tr>
        <tr>
            <td>小2</td>
            <td>我是小2，一个小2，两个小2</td>
        </tr>
    </table>

    <h2>10.border——表格边框样式</h2>
    <table border="1" style="border-collapse: separate;border: 3px solid green;">
        <tr>
            <th>名字</th>
            <th>介绍</th>
        <tr>
        <tr>
            <td>小1</td>
            <td>我是小1</td>
        </tr>
        <tr>
            <td>小2</td>
            <td>我是小2，一个小2，两个小2</td>
        </tr>
    </table>

</body>

</html>
```