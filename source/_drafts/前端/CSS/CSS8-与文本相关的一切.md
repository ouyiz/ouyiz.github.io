# 1. `font-family` —— 字体类型
**作用**：设置元素文本的字体类型。  
**语法**：
```css
selector {
    font-family: 字体名称或者字体族;
}
```
- 可以使用常见的字体名称，如：
	- `"Arial"`（无衬线字体）
	- `"Times New Roman"`（衬线字体）
	- `"Courier New"`（等宽字体）
	- `"Georgia"`（衬线字体）
	- `"Verdana"`（无衬线字体）
- CSS 提供了五个通用字体族，浏览器会根据系统中可用的字体来选择最合适的字体。它们包括：
	- `serif`：衬线字体（如 Times, Georgia）
	- `sans-serif`：无衬线字体（如 Arial, Verdana）
	- `monospace`：等宽字体（如 Courier New）
	- `cursive`：手写体（如 Comic Sans MS）
	- `fantasy`：幻想字体（如 Impact）
- 一些系统字体也可以作为字体名称使用，如：
	- `-apple-system`：针对苹果设备（如 macOS 或 iOS）
	- `Segoe UI`：针对 Windows 系统
	- `Roboto`：常用于 Android 系统和现代浏览器
- 如果你使用了自定义字体（例如通过 `@font-face` 引入的字体），你可以在 `font-family` 中使用该自定义字体的名称。

**示例**：
如果浏览器没有找到 `"Helvetica"`，则尝试使用 `"Arial"`，如果还是没有，则使用通用的无衬线字体（`sans-serif`）：
```css
p {
    font-family: "Helvetica", "Arial", sans-serif;
}
```
自定义字体：
```CSS
@font-face {
    font-family: "MyCustomFont";
    src: url("mycustomfont.woff2") format("woff2");
}

h2 {
    font-family: "MyCustomFont", Arial, sans-serif;
}

```
**界面示例：**
<img src="file-20250417141517046.png" style="width: 300px; height: auto;">

# 2. `font-size` —— 字体大小
**作用**：设置元素文本的字体大小。  
**语法**：
```css
selector {
    font-size: 字体大小;
}
```
单位示例：

|单位|描述|示例|备注|
|---|---|---|---|
|`px`|像素，绝对单位，常用于屏幕显示|`font-size: 16px;`|不受父元素字体大小影响|
|`pt`|磅，绝对单位，通常用于打印|`font-size: 12pt;`|1pt = 1/72 英寸|
|`em`|相对于父元素的字体大小|`font-size: 2em;`|1em 是当前元素字体大小的倍数|
|`rem`|相对于根元素的字体大小|`font-size: 1.5rem;`|1rem 是根元素（`<html>`）字体大小的倍数|
|`%`|相对于父元素的字体大小|`font-size: 120%;`|父元素字体的 120%|
|`vw`|视口宽度的百分比|`font-size: 5vw;`|1vw 是视口宽度的 1%|
|`vh`|视口高度的百分比|`font-size: 5vh;`|1vh 是视口高度的 1%|
|`in`|英寸，绝对单位，常用于打印|`font-size: 1in;`|1in = 2.54 厘米|
|`cm`|厘米，绝对单位|`font-size: 2cm;`|使用物理长度单位|
|`mm`|毫米，绝对单位|`font-size: 10mm;`|使用物理长度单位|
|`larger`|比当前字体大，通常是当前字体大小的 1.2 倍|`font-size: larger;`|相对大小|
|`smaller`|比当前字体小，通常是当前字体大小的 0.8 倍|`font-size: smaller;`|相对大小|

**示例**：
```css
h1 {
    font-size: 24px;
}
```



# 3. `font-weight` —— 字体加粗
**作用**：设置元素文本的粗细。  
**语法**：
```css
selector {
    font-weight: 字体粗细;
}
```
字体粗细值：
- 数值
	- `100` 至 `900`：表示字体的粗细程度，值越大字体越粗。通常以 `100` 为最细，`900` 为最粗。值之间通常以 `100` 为间隔
- 关键词:
	- `normal`：表示正常字体粗细，等同于 `400`。
	- `bold`：表示加粗字体，等同于 `700`。
	- `bolder`：表示比父元素字体更粗的字体。
	- `lighter`：表示比父元素字体更细的字体。
**示例**：
```css
strong {
    font-weight: bold;
}
p {
     font-weight: 700; /* 加粗 */
}
```
**界面示例：**
<img src="file-20250417141650112.png" style="width: 300px; height: auto;">

# 4. `font-style` —— 字体样式
**作用**：设置元素文本的样式，如斜体或正常。  
**语法**：
```css
selector {
    font-style: 字体样式;
}
```

| 属性值               | 说明                                        |
| ----------------- | ----------------------------------------- |
| `normal`          | 正常字体（默认，不倾斜、不变形）。                         |
| `italic`          | 斜体（通常是字体专门设计的斜体样式）。                       |
| `oblique`         | 倾斜体（通过数学变换将字体倾斜，视觉上类似斜体）。                 |
| `oblique <angle>` | 指定倾斜的角度（例如：`oblique 20deg`），CSS3特性，兼容性较差。 |
| `inherit`         | 继承父元素的 `font-style`。                      |
| `initial`         | 将 `font-style` 恢复为默认值 `normal`。           |
| `unset`           | 如果父元素有定义就继承，否则设置为默认 `normal`。             |

**示例**：
```css
em {
    font-style: italic;
}
```
**界面示例：**
<img src="file-20250417142054911.png" style="width: 300px; height: auto;">



# 5. `text-transform` —— 文本变换
**作用**：控制文本的变换方式，如大写、小写或首字母大写。  
**语法**：
```css
selector {
    text-transform: 变换方式;
}
```

|取值|说明|
|---|---|
|`none`|默认值，不进行任何大小写变换，保持原样。|
|`capitalize`|每个单词的首字母大写，其余字母保持原样。|
|`uppercase`|将所有字母转换为大写。|
|`lowercase`|将所有字母转换为小写。|
|`full-width`|将英文字母和数字转换为全角字符（在部分东亚文字环境有效）。|
|`inherit`|继承父元素的 `text-transform` 属性值。|
|`initial`|将属性设置为初始默认值（即 `none`）。|
|`unset`|如果有继承则继承父级值，否则等同于 `initial`。|

**示例**：

```css
h2 {
    text-transform: uppercase;
}
```

**界面示例：**
<img src="file-20250417142748636.png" style="width: 300px; height: auto;">

# 6. `text-align` —— 文本对齐

**作用**：`text-align` 是控制文本在父级容器的水平对齐方式，对齐的基准永远是父容器的内容区域（即 `padding` 内部的部分）。  
**语法**：
```css
selector {
    text-align: 对齐方式;
}
```

| `text-align` 值  | 作用                     | 相对于谁对齐？           |
| --------------- | ---------------------- | ----------------- |
| `left`（左对齐）     | 文本靠容器的左侧对齐。            | **父级容器的左边缘（内容区）** |
| `right`（右对齐）    | 文本靠容器的右侧对齐。            | **父级容器的右边缘（内容区）** |
| `center`（居中对齐）  | 文本在容器中水平居中。            | **父级容器的中轴线**      |
| `justify`（两端对齐） | 文本左右两边都对齐，自动拉伸间距。      | **父级容器的左右边缘**     |
| `start` / `end` | 根据文本书写方向（LTR/RTL）自动对齐。 | **父级容器的起点或终点边缘**  |

**示例**：
```css
p {
    text-align: center;
}
```

**界面示例：**
<img src="file-20250417143700805.png" style="width: 300px; height: auto;">


# 7. `line-height` —— 行高

**作用**：设置文本的行高，通常用于控制行间距。  
**语法**：
```css
selector {
    line-height: 行高;
}
```

| **取值**     | **描述**                                                                                                                  |
| ---------- | ----------------------------------------------------------------------------------------------------------------------- |
| **数值（数字）** | 如 `1.5`，它是一个倍数值，表示行高是字体大小的倍数。  <br>例如，`line-height: 1.5;` 意味着行高是字体大小的 1.5 倍。                                            |
| **长度值**    | 如 `px`, `em`, `rem` 等单位的长度值，表示具体的像素或其他单位的高度。例如，`line-height: 24px;`。                                                    |
| **百分比**    | 以字体大小的百分比来设定行高。例如，`line-height: 150%;` 表示行高是字体大小的 150%。                                                                 |
| **关键字**    | `normal` 和 `inherit` 两个关键字：  <br>- `normal`（默认值）：表示浏览器使用默认的行高值，通常是字体大小的 1.2 倍。  <br>- `inherit`：继承父元素的 `line-height` 值。 |


**示例**：
```css
p {
    line-height: 1.5;
}
```

**界面示例：**
<img src="file-20250417144250518.png" style="width: 300px; height: auto;">


# 8. `color` —— 文本颜色

**作用**：设置元素文本的颜色。  
**语法**：
```css
selector {
    color: 颜色值;
}
```

**示例**：
```css
h3 {
    color: red;
}
```



# 9. `text-decoration` —— 文本装饰

**作用**：设置文本的装饰，如下划线、删除线等。  
**语法**：
```css
selector {
    text-decoration: 装饰类型;
}
```
- **none** - 无装饰。去掉文本的任何装饰效果。
- **underline** - 下划线。给文本添加下划线。
- **overline** - 上划线。给文本添加上划线。
- **line-through** - 删除线。给文本添加删除线。
- **blink** - 闪烁（已废弃）。让文本闪烁显示，但大多数浏览器已经不支持该效果。

**示例**：

```css
a {
    text-decoration: underline;
}
```

**界面示例：**
<img src="file-20250417144624947.png" style="width: 300px; height: auto;">



# 10. `letter-spacing` —— 字符间距

**作用**：设置文本字符之间的间距。  
**语法**：
```css
selector {
    letter-spacing: 间距值;
}
```
- 间距值：设置字符之间的间距，可以使用像素（px）、em、rem、pt 等单位。例如：
    - `letter-spacing: 2px;`：字母间距为 2 像素
    - `letter-spacing: 0.1em;`：字母间距为 0.1 个字母宽度
    - `letter-spacing: 1rem;`：字母间距为 1 个根元素字体大小的长度

**示例**：
```css
h4 {
    letter-spacing: 2px;
}
```


# 11. `word-spacing` —— 单词间距

**作用**：设置文本单词之间的间距。  
**语法**：
```css
selector {
    word-spacing: 间距值;
}
```
- 间距值：设置单词之间的间距，可以使用像素（px）、em、rem、pt 等单位。例如：
    - `letter-spacing: 2px;`：单词间距为 2 像素
    - `letter-spacing: 0.1em;`：单词间距为 0.1 个字母宽度
    - `letter-spacing: 1rem;`：单词间距为 1 个根元素字体大小的长度

**示例**：
```css
p {
    word-spacing: 5px;
}
```



# 12. `text-shadow` —— 文本阴影

**作用**：为文本添加阴影效果。  

**语法**：
```css
selector {
    text-shadow: 阴影偏移水平 阴影偏移垂直 阴影模糊半径 阴影颜色;
}
```
- **阴影偏移水平**（必需）：决定阴影的水平偏移量。可以是正值或负值，正值表示阴影向右偏移，负值表示阴影向左偏移。
- **阴影偏移垂直**（必需）：决定阴影的垂直偏移量。可以是正值或负值，正值表示阴影向下偏移，负值表示阴影向上偏移。
- **阴影模糊半径**（可选）：决定阴影的模糊程度。值越大，阴影越模糊；如果不指定，阴影将是锐利的。
- **阴影颜色**（可选）：指定阴影的颜色。如果不指定，默认为黑色。

**示例**：
```css
h1 {
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}
```

**界面示例：**
<img src="file-20250417145525744.png" style="width: 300px; height: auto;">


# 13. `text-indent` —— 文本缩进

**作用**：设置段落首行的缩进。  

**语法**：
```css
selector {
    text-indent: 缩进值;
}
```
- 长度单位
	- **px**（像素）：常见的单位之一，设置具体的像素值。
	- **em**（相对于元素的字体大小）：通常用于相对的缩进，字体大小变化时也会相应变化。
	- **rem**（相对于根元素的字体大小）：类似于 em，但它是基于根元素的字体大小。
	- **%**（百分比）：相对于元素的宽度来设置缩进值。
	- **pt**（磅）：另一种长度单位，通常用于打印或页面设计中。
- 关键字
	- **`initial`**：设置为默认值，通常为0。
	- **`inherit`**：从父元素继承缩进值。
**示例**：
```css
p {
    text-indent: 20px;
}
```

**界面示例：**
<img src="file-20250417145850290.png" style="width: 300px; height: auto;">

# 14. `white-space` —— 白空格控制

**作用**：设置如何处理文本中的空格。  

**语法**：
```css
selector {
    white-space: 空格处理方式;
}
```

| 取值             | 描述                                     |
| -------------- | -------------------------------------- |
| `normal`       | 默认，空格和换行符会被合并，文本会自动换行。                 |
| `nowrap`       | 文本不换行，空格和换行符被忽略。                       |
| `pre`          | 保留空格和换行符的原始格式。                         |
| `pre-wrap`     | 保留空格和换行符的原始格式，超出容器宽度时会自动换行。            |
| `pre-line`     | 保留空格，合并多余的空格，换行符被保留，超出容器宽度时会自动换行。      |


**示例**：
```css
pre {
    white-space: pre-wrap;
}
```

**界面示例：**
<img src="file-20250417150654317.png" style="width: 300px; height: auto;">

# 15. `text-overflow` —— 文本溢出
**作用**：设置文本溢出时的显示方式，常见的值有省略号或裁剪。  

**语法**：
```css
selector {
    text-overflow: 溢出处理方式;
}
```

| 取值         | 描述                    |
| ---------- | --------------------- |
| `clip`     | 默认值，文本超出容器时会被剪切掉。     |
| `ellipsis` | 文本超出容器时会显示省略号（`...`）。 |

**示例**：
```css
div {
    text-overflow: ellipsis;
}
```



# 16. `direction` —— 文本方向
**作用**：设置文本的书写方向。  

**语法**：
```css
selector {
    direction: 方向;
}
```

| 属性值       | 说明                                     |
| --------- | -------------------------------------- |
| `ltr`     | 从左到右（Left To Right），**默认值**，适用于英文、中文等。 |
| `rtl`     | 从右到左（Right To Left），适用于阿拉伯语、希伯来语等。     |
| `inherit` | 继承父元素的 `direction` 值。                  |
**示例**：
```css
p {
    direction: rtl;
}
```

**界面示例：**
<img src="file-20250417151326215.png" style="width: 600px; height: auto;">


# 17. `writing-mode` —— 文字列模式

**作用**：设置文本的排版方向。  

**语法**：
```css
selector {
    writing-mode: 排版方式;
}
```

|属性值|说明|示例效果|
|---|---|---|
|`horizontal-tb` （默认值）|**水平方向从左到右，垂直方向从上到下**（横排，常用于中文/英文）。|🡢 横向排列，从上到下换行。|
|`vertical-rl`|**垂直方向从上到下，行从右到左**（竖排，中文/日文书籍常用）。|🡣 从右往左一列列排布。|
|`vertical-lr`|**垂直方向从上到下，行从左到右**（较少见）。|🡣 从左往右一列列排布。|
|`sideways-rl`|文本**横着旋转90度**，从右到左排列，行从上到下。|横着的文字，行从上往下。|
|`sideways-lr`|文本**横着旋转90度**，从左到右排列，行从上到下。|横着的文字，行从上往下。|
|`inherit`|继承父元素的 `writing-mode`。|跟随父级设置。|
**示例**：
```css
p {
    writing-mode: vertical-rl;
}
```

**界面示例：**
<img src="file-20250417151732139.png" style="width: 400px; height: auto;">

# 18.本节代码
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Font Family Example</title>
    <style>
        /* 引入自定义字体 */
        @font-face {
            font-family: "MyCustomFont";
            src: url("fonts/BRUSHSCI.TTF") format("truetype");
        }


        /* 设置 p 元素的字体 */
        #para1 {
            font-family: "Helvetica", "Arial", sans-serif;
            font-size: 16px;
            color: #333;
        }


        /* 设置 h2 元素的字体 */
        #heading2 {
            font-family: "MyCustomFont", Arial, sans-serif;
            font-size: 24px;
            color: #16a085;
        }

        /* 设置 h3 元素的字体 */
        #heading3 {
            font-family: "Verdana", sans-serif;
            font-size: 20px;
            color: #2980b9;
        }
    </style>
</head>
<body>

    <!-- 设置字体类型 -->
    <h1>font-family —— 字体类型示例</h1>
    <p id="para1">这段的格式：font-family: "Helvetica", "Arial", sans-serif.</p>
    <h2 id="heading2">这段使用上传的字体：abc</h2>
    <h3 id="heading3">这段的格式：This is a Heading 3 (Verdana)</h3>

    <!-- 设置字体粗细 -->
    <h1>font-weight —— 字体加粗示例</h1>
    <p style="font-weight: normal; font-size: 24px; color: #16a085;">
        正常效果
    </p>
    <p style="font-weight: bold; font-size: 24px; color: #16a085;">
        加粗效果
    </p>
    <p style="font-weight: 600; font-size: 24px; color: #2980b9;">
        600的效果
    </p>
    <p style="font-weight: lighter; font-size: 24px; color: #333;">
        lighter的效果
    </p>

  

    <!-- 设置字体样式 -->
    <h1>font-style —— 字体样式示例</h1>
    <p style="font-style: normal; font-size: 24px; color: #16a085;">
        normal（正常字体效果）
    </p>
    <p style="font-style: italic; font-size: 24px; color: #2980b9;">
        italic（斜体效果）
    </p>
    <p style="font-style: oblique; font-size: 24px; color: #e74c3c;">
        oblique（倾斜体效果）
    </p>

  

    <!-- 设置文本转换方式 -->
    <h1>text-transform —— 文本大小写转换示例</h1>
    <p style="text-transform: none; font-size: 24px; color: #16a085;">
        原样显示：This is Sample Text.
    </p>
    <p style="text-transform: uppercase; font-size: 24px; color: #16a085;">

        转为大写：This is Sample Text.

    </p>

    <p style="text-transform: lowercase; font-size: 24px; color: #2980b9;">
        转为小写：This is Sample Text.
    </p>
    <p style="text-transform: capitalize; font-size: 24px; color: #333;">

        首字母大写：this is sample text.
    </p>

  

    <!-- 设置文本对齐方式 -->

    <h1>text-align —— 文本对齐示例</h1>
    <p style="text-align: left; font-size: 20px; color: #2c3e50;">
        左对齐效果（left）
    </p>
    <p style="text-align: right; font-size: 20px; color: #2c3e50;">
        右对齐效果（right）
    </p>

    <p style="text-align: center; font-size: 20px; color: #16a085;">
        居中对齐效果（center）
    </p>
    <p style="text-align: justify; font-size: 20px; width: 400px; color: #2980b9;">
        两端对齐效果（justify）。文本将会自动拉伸，使得左右两边都对齐，段落中每行间距会自动调节，适合正文排版。
    </p>

  

    <p style="text-align: start; font-size: 20px; color: #8e44ad;">
        start 对齐效果（start，基于文本书写方向，默认是左对齐）
    </p>


    <p style="text-align: end; font-size: 20px; color: #e67e22;">
        end 对齐效果（end，基于文本书写方向，默认是右对齐）
    </p>

    <!-- 设置行高 -->
    <h1>line-height —— 行高示例</h1>
    <p style="line-height: 1.5; font-size: 20px; color: #2c3e50;">
        行高为 1.5（1.5 倍字体大小），提升文本行间距。<br>
        行高为 1.5（1.5 倍字体大小），提升文本行间距。
    </p>

    <p style="line-height: 4; font-size: 16px; color: #e67e22;">
        行高为 4（4 倍字体大小），提升文本行间距。<br>
        行高为 4（4 倍字体大小），提升文本行间距。
    </p>

    <p style="line-height: 30px; font-size: 20px; color: #16a085;">
        行高为 30px（固定高度），每行之间的间距为 30px。<br>
        行高为 30px（固定高度），每行之间的间距为 30px。
    </p>

  

    <p style="line-height: 120%; font-size: 20px; color: #2980b9;">
        行高为 120%（字体大小的 120%），使得文本更加舒适可读。<br>
        行高为 120%（字体大小的 120%），使得文本更加舒适可读。
    </p>

    <p style="line-height: normal; font-size: 20px; color: #8e44ad;">
        行高为 normal（默认值），根据浏览器默认设置调整行距。<br>
        行高为 normal（默认值），根据浏览器默认设置调整行距。
    </p>

    <!-- 设置文本装饰 -->
    <h1>text-decoration —— 文本装饰示例</h1>
    <p style="text-decoration: none; font-size: 20px; color: #2c3e50;">
        无装饰效果（none）
    </p>
    <p style="text-decoration: underline; font-size: 20px; color: #16a085;">
        下划线效果（underline）
    </p>
    <p style="text-decoration: overline; font-size: 20px; color: #2980b9;">
        上划线效果（overline）
    </p>
    <p style="text-decoration: line-through; font-size: 20px; color: #8e44ad;">
        删除线效果（line-through）
    </p>
    <p style="text-decoration: underline overline; font-size: 20px; color: #e67e22;">
        同时使用上下划线效果（underline overline）
    </p>

  

    <!-- 设置文本阴影 -->
    <h1>text-shadow —— 文本阴影示例</h1>
    <p style="text-shadow: 2px 2px 5px #888888;">
        这是一个带有阴影的文本，阴影偏移为2px，垂直和水平方向，模糊半径为5px，颜色为灰色。
    </p>
    <p style="text-shadow: -2px -2px 7px blue;">
        这是一个带有阴影的文本，阴影偏移为-2px，-2px，模糊半径为7px，颜色为半透明的黑色。
    </p>

    <p style="text-shadow: 5px 5px 0px red;">
        这是一个带有红色阴影的文本，阴影偏移为5px，5px，模糊半径为0px（没有模糊效果）。
    </p>    


    <!-- 设置文本缩进 -->
    <h1>text-indent —— 文本缩进示例</h1>
    <p style="text-indent: 80px;">
        这是一个80px的文本缩进。
    </p>
    <p style="text-indent: 2em;">
        这是一个2em的文本缩进，基于当前字体大小。
    </p>
    <p style="text-indent: 10%;">
        这是一个相对于容器宽度的文本缩进，10%。
    </p>
    <p style="text-indent: 0;">
        这是没有缩进的文本。
    </p>
    <p style="text-indent: inherit;">
        这是从父元素继承缩进的文本。
    </p>

    <!-- 设置文本换行 -->
    <h1>white-space —— 文本换行示例</h1>
    <p style="white-space: normal;">
        这是一个normal例子，     其中的    空格会被合并，文本会根据容器的宽度自动换行。文本会根据容器的宽度自动换行文本会根据容器的宽度自动换行
    </p>
    <p style="white-space: nowrap;">
        这是一个nowrap例子，     其中的    空格会被忽略，文本不会换行，所有内容会显示在一行。所有内容会显示在一行所有内容会显示在一行所有内容会显示在一行
    </p>
    <p style="white-space: pre;">
        这是一个pre例子，     其中的    空格和
        换行符都会被保留。
    </p>
    <p style="white-space: pre-wrap;">
        这是一个pre-wrap例子，     其中的    空格和换行符都会被保留，文本会根据容器宽度换行。
    </p>
    <p style="white-space: pre-line;">
        这是一个pre-line例子，     其中的    空格会合并，换行符会保留，文本会根据容器宽度换行。
    </p>

    <!-- 设置文本溢出 -->
    <h1>text-overflow —— 文本溢出示例</h1>
    <p style="width: 200px; white-space: nowrap; overflow: hidden; text-overflow: clip;">
        这是一个很长的文本，超过容器宽度后会被剪切掉。
    </p>
    <p style="width: 200px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis;">
        这是一个很长的文本，超过容器宽度后会显示省略号...
    </p>

    <!-- 设置文本方向 -->
    <h1>direction —— 文本方向示例</h1>
    <p style="direction: ltr;">
        This text is left-to-right.
    </p>
    <p style="direction: rtl;">
        هذا النص من اليمين إلى اليسار.
    </p>
    
    <!-- 设置排版方向 -->
    <h1>writing-mode —— 文本排版方向示例</h1>
    <p style="writing-mode: horizontal-tb; border: 1px solid #ccc; padding: 10px; width: 200px;">
        horizontal-tb：默认横排，从左到右，换行从上到下。
    </p>
    <p style="writing-mode: vertical-rl; border: 1px solid #ccc; padding: 10px; height: 100px;">
        vertical-rl：竖排，从上到下，列从右到左。
    </p>
    <p style="writing-mode: vertical-lr; border: 1px solid #ccc; padding: 10px; height: 100px;">
        vertical-lr：竖排，从上到下，列从左到右。
    </p>
    <p style="writing-mode: sideways-rl; border: 1px solid #ccc; padding: 10px; height: 100px;">
        sideways-rl：文字横向旋转90度，行从右向左排布。
    </p>
    <p style="writing-mode: sideways-lr; border: 1px solid #ccc; padding: 10px; height: 100px;">
        sideways-lr：文字横向旋转90度，行从左向右排布。
    </p>
</body>
</html>
```