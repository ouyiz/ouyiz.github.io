# CSS 盒子模型全面解析

## 一、盒子模型基础概念

CSS盒子模型(Box Model)是CSS布局的核心概念，它描述了文档中每个元素被看作一个矩形的盒子，这个盒子由内容(content)、内边距(padding)、边框(border)和外边距(margin)组成。
![](file-20250510162203632.png)

| 组成部分        | 作用                | 相关属性                                                     | 默认值    |
| ----------- | ----------------- | -------------------------------------------------------- | ------ |
| content：内容  | 实际显示的内容区域（文字、图片等） | `width`, `height`                                        | 浏览器决定  |
| padding：内边距 | 内容与边框之间的空白区域      | `padding`, `padding-top/right/bottom/left`               | `0`    |
| border：边框   | 围绕padding的边框      | `border`, `border-width`, `border-style`, `border-color` | `none` |
| margin：外边距  | 元素之间的外边距，推开其他元素   | `margin`, `margin-top/right/bottom/left`                 | `0`    |
界面示例：
<img src="file-20250510164217203.png" style="width: 200px; height: auto;">
## 二、盒子模型的两种模式
#### 1.标准盒模型（content-box）
- `width` 只设置 content 区域的宽度。
- 总元素的高度=`width`+顶部填充+底部填充+上边框+下边框+上边距+下边距
```css
box-sizing: content-box;
```
示例图：
<img src="file-20250510190035972.png" style="width: 200px; height: auto;">

#### 2.IE盒模型（border-box）
- `width` 包含 content + padding + border。
- 实际宽度 = `width`+上边距+下边距
```css
box-sizing: border-box;
```
示例图：
<img src="file-20250510190311110.png" style="width: 200px; height: auto;">

## 三、border边框
`border `用于设置元素边框的宽度、样式和颜色，能够美化布局、分隔内容、增强交互效果等。
#### 1.简写属性
**作用**：设置元素所有四条边的边框，包括宽度、样式和颜色
**语法**：
```css
selector {
    border: width style color;
}
```
- `width`：边框宽度，如 `1px`、`thin`、`medium`、`thick`。
- `style`：边框样式，如 `solid`、`dashed`、`dotted`、`double`、`groove`、`ridge`、`inset`、`outset`、`none`、`hidden`。
- `color`：边框颜色，如 `red`、`#ff0000`、`rgb(255,0,0)` 等。
 **示例**：
```css
div {
    border: 2px dashed green;
}
```

#### 2.边框方向属性：`border-top/right/bottom/left`
 **作用**：分别设置上、右、下、左边框。
 **语法**：
```css
selector {
    border-top: width style color;
    border-right: width style color;
    border-bottom: width style color;
    border-left: width style color;
}
```

 **示例**：
```css
div {
    border-top: 3px solid red;
    border-left: 3px dotted blue;
}
```

####  3.子属性：`border-width`, `border-style`, `border-color`
**作用**：分别设置边框的宽度、样式和颜色。
**语法**：
```css
selector {
    border-width: top right bottom left;
    border-style: top right bottom left;
    border-color: top right bottom left;
}
```
- 可以设置 1~4 个值：
    - 1个：四边相同
    - 2个：上下 / 左右
    - 3个：上 / 左右 / 下
    - 4个：上 / 右 / 下 / 左
**示例**：
```css
div {
    border-width: 1px 2px 3px 4px;
    border-style: solid dashed dotted double;
    border-color: red green blue black;
}
```

#### 4.边框宽度：`border-width` 与 `border-*-width`
**作用**：设置边框的宽度。
**语法**：
```css
selector {
    border-width: [top] [right] [bottom] [left];
    border-top-width: value;
    border-right-width: value;
    ...
}
```
合法值：
- 具体数值：`2px`、`0.5em` 等
- 关键字：`thin`、`medium`（默认） 、`thick`
**示例**：
```css
p {
    border-width: 5px 10px;
    border-left-width: 20px;
}
```
示例图：
<img src="file-20250512231653734.png" style="width: 300px; height: auto;">

#### 5.边框样式：`border-style` 与 `border-*-style`
**作用**：定义边框的线条样式。
**语法**：
```css
selector {
    border-style: [top] [right] [bottom] [left];
    border-top-style: value;
    ...
}
```
常见值：
- `none`：无边框（与 `hidden` 类似但不能用于表格冲突解决）
- `solid`：实线
- `dashed`：虚线
- `dotted`：点线
- `double`：双线
- `groove`：3D凹槽边框（依赖颜色）
- `ridge`：3D凸起边框
- `inset`：内嵌（看起来凹进去）
- `outset`：外凸（看起来凸出来）
**示例**：
```css
div {
    border-style: solid dashed dotted double;
}
```
示例图：
<img src="file-20250510191052939.png" style="width: 200px; height: auto;">

#### 6.边框颜色：`border-color` 与 `border-*-color`
**作用**：设置边框颜色。
**语法**：
```css
selector {
    border-color: [top] [right] [bottom] [left];
    border-top-color: value;
    ...
}
```
**合法值**：
- 颜色值：`red`、`#000`、`rgba(0,0,0,0.5)` 等
- 默认值是 `color` 属性当前值
**示例**：
```css
div {
    border-color: red green blue black;
}
```
示例图：
<img src="file-20250512231748545.png" style="width: 300px; height: auto;">

#### 7.`border-radius`（圆角边框）
**作用**：设置边框的圆角。
**语法**：
```css
selector {
    border-radius: [top-left] [top-right] [bottom-right] [bottom-left];
}
```
- 单值：四角相同
- 两值：左上/右下，右上/左下
- 四值：依次设置四个角
- 支持椭圆：可写作 `border-radius: h-radius / v-radius`
- 单独属性：
	- `border-top-left-radius`
	- `border-top-right-radius`
	- `border-bottom-left-radius`
	- `border-bottom-right-radius`
**示例**：
```css
div {
    border-radius: 10px 20px 30px 40px;
}
img {
    border-radius: 50%;
}
```
示例图：
<img src="file-20250512231833777.png" style="width: 300px; height: auto;">


## 四、outline轮廓
`outline` 是一种绘制在元素边框之外的线条，常用于可访问性增强和交互反馈，如键盘聚焦等。它不占据空间，不会影响布局。
#### 1.简写属性
**作用**：设置元素轮廓的宽度、样式和颜色  
**语法**：
```css
selector {
    outline: width style color;
}
```
- `width`：轮廓宽度，如 `1px`、`thin`、`medium`、`thick`
- `style`：轮廓样式，如 `solid`、`dashed`、`dotted`、`double`、`none`
- `color`：轮廓颜色，如 `blue`、`#00f`、`rgba(0, 0, 255, 0.5)`  
    **示例**：
```css
button {
    outline: 2px dotted blue;
}
```

#### 2.子属性：`outline-width`、`outline-style`、`outline-color`
**作用**：分别设置轮廓的宽度、样式和颜色  
**语法**：
```css
selector {
    outline-width: value;
    outline-style: value;
    outline-color: value;
}
```
合法值：
- `outline-width`：`thin`、`medium`、`thick` 或具体值如 `2px`
- `outline-style`：`none`、`solid`、`dashed`、`dotted`、`double` 等
- `outline-color`：可设置任意颜色值，也可以为 `invert`（反转颜色）  
    **示例**：
```css
input:focus {
    outline-width: 3px;
    outline-style: solid;
    outline-color: green;
}
```

#### 3.区别 border 与 outline

| 属性        | 是否占空间 | 是否影响布局 | 支持圆角 | 常用场景      |
| --------- | ----- | ------ | ---- | --------- |
| `border`  | ✅     | ✅      | ✅    | 内容装饰、布局   |
| `outline` | ❌     | ❌      | ❌    | 焦点提示、辅助功能 |

- `outline` 始终绘制在元素**外侧**，不会改变元素的大小。  
- `outline` 不支持每边分别设置，仅支持统一设置。
#### 4.`outline-offset`：轮廓偏移
**作用**：设置轮廓与元素边缘之间的距离（正值外移，负值内缩）  
**语法**：
```css
selector {
    outline-offset: value;
}
```
**示例**：
```css
a:focus {
    outline: 2px solid red;
    outline-offset: 4px;
}
```

#### 6.示例
```html
<style>
    button {
      padding: 10px 20px;
      font-size: 16px;
      border: 2px solid #ccc;
      background-color: white;
      cursor: pointer;
    }

    /* 鼠标悬停时显示轮廓 */
    button:hover {
      outline: 3px dashed blue;
      outline-offset: 20px;
    }
  </style>
```

```html
<h2>四.outline轮廓</h2>
    <p>鼠标悬停时显示 Outline</p>
    <button>把鼠标移上来试试</button>
```

**界面示例图：**
<img src="file-20250512232457600.png" style="width: 200px; height: auto;">
**盒子模型图：**
<img src="file-20250512232655043.png" style="width: 200px; height: auto;">

## 五、margin外边距
#### 1. 简写属性
**作用**：设置元素四个方向（上、右、下、左）的外边距  
**语法**：
```css
selector {
    margin: top right bottom left;
}
```
- 可设置 1~4 个值：
    - 1个值：四边相同
    - 2个值：上/下，左/右
    - 3个值：上，左/右，下
    - 4个值：上，右，下，左
- 值类型：
    - 长度值：`px`、`em`、`rem` 等，如 `10px`
    - 百分比：相对于包含块宽度，如 `5%`
    - `auto`：浏览器自动计算，一般用于水平居中
**示例**：
```css
div {
    margin: 10px 20px 30px 40px;
}
```
#### 2. 方向属性：`margin-top` / `margin-right` / `margin-bottom` / `margin-left`
**作用**：分别设置四个方向上的外边距  
**语法**：
```css
selector {
    margin-top: value;
    margin-right: value;
    margin-bottom: value;
    margin-left: value;
}
```
**示例**：

```css
div {
    margin-top: 20px;
    margin-left: 10%;
}
```
#### 3. 特殊值 `auto`
**作用**：让浏览器根据布局自动计算外边距，常用于水平居中  
**语法**：
```css
selector {
    margin: auto;
}
```
**说明**：
- 块级元素设置固定宽度 + `margin: auto` 可实现水平居中
- 对于上下方向，`auto` 一般无实际效果

**示例**：
```css
div {
    width: 300px;
    margin: 0 auto;
}
```

#### 4. 外边距合并（Margin Collapsing）
**作用**：垂直方向上相邻块级元素之间的 margin 不会相加，而是合并为其中的最大值（正值）或按规则合并（正负值）。
**语法**：
- 两个相邻的块级元素之间的 margin 会发生合并，只保留较大者。
- 当一个块级父元素的 `margin-top` 与其第一个块级子元素的 `margin-top` 相遇时，可能会合并。
- 父元素的 `margin-bottom` 和其最后一个子元素的 `margin-bottom` 也可能发生合并。
- 合并规则详解
	- 两个正值：保留最大值
	- 一个正值+一个负值：结果为两者相加，可能为负
	- 两个负值：保留绝对值最大的负值

**示例**：
```css
.box1 {
    background-color: lightblue;
    height: 100px;
    margin-bottom: 30px;
}

.box2 {
    background-color: lightgreen;
    height: 100px;
    margin-top: -40px;
}

```

```html
<div class="box1">Box 1</div>
<div class="box2">Box 2</div>
```

示例图：
<img src="file-20250513115106811.png" style="width: 200px; height: auto;">
#### 5. 百分比值
**说明**：`margin` 的百分比是 基于父元素宽度 计算，而不是自身或高度  
**示例**：
`.box` 的 `margin-top: 10%` 是 `40px`，因为父容器 `.container` 的宽度是 `400px`。
```css
.container {
    width: 400px;
    height: 300px;
    background-color: #eee;
}
.box {
    width: 100px;
    height: 100px;
    margin-top: 10%;
    background-color: orange;
}
```

#### 6. 正负值
**说明**：`margin` 可设置为负值，用于制造元素之间的重叠或位置调整  
**示例**：
```css
div {
    margin-top: -20px;
}
```

## 六、padding填充
#### 1.简写属性
**作用**：设置元素四个方向（上右下左）的内边距。
**语法**：
```css
selector {
    padding: top right bottom left;
}
```
- 可以设置 1~4 个值：
    - 1个：四边相同
    - 2个：上下 / 左右
    - 3个：上 / 左右 / 下
    - 4个：上 / 右 / 下 / 左
**示例**：
```css
div {
    padding: 10px 20px 15px 5px;
}
```

#### 2.方向属性：padding-top/right/bottom/left
**作用**：分别设置元素上、右、下、左的内边距。
**语法**：
```css
selector {
    padding-top: value;
    padding-right: value;
    padding-bottom: value;
    padding-left: value;
}
```
- `value` 可以为 `px`、`em`、百分比（相对于父元素宽度）等单位。
**示例**：
```css
div {
    padding-top: 20px;
    padding-left: 5%;
}
```

#### 3.值类型与注意事项
**合法值**：
- 绝对长度：如 `10px`、`2em`
- 百分比：相对于**父元素的宽度**
- 不能为负值，浏览器会忽略
**注意**：
- `padding` 会撑大盒子尺寸，除非使用 `box-sizing: border-box;` 令其包含在 `width` 和 `height` 内。
**示例**：
```css
.box {
    padding: 20px;
    box-sizing: border-box;
}
```

## 七、本节代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Background Repeat and Position Demo</title>
    <style>
        .box1 {
            width: 100px;
            height: 100px;
            border: 20px solid red;
            padding: 10px;
            margin: 30px;
        }
        .box2-1 {
            width: 100px;
            height: 100px;
            border: 20px solid yellow;
            padding: 10px;
            margin: 30px;
            box-sizing: content-box;
        }
        .box2-2 {
            width: 100px;
            height: 100px;
            border: 20px solid green;
            padding: 10px;
            margin: 30px;
            box-sizing: border-box;
        }
        .box3-1 {
            width: 300px;
            height: 20px;
            margin: 10px;
            padding: 10px;
            border-width: 5px;
            border-color: #333;
        }
        .box3-2 {
            border-style: solid;
            width: 300px;
            height: 40px;
            margin: 10px;
            padding: 10px;
            border-color: #333;
        }
        .box3-3 {
            border-style: solid;
            width: 300px;
            height: 40px;
            margin: 10px;
            padding: 10px;
            border-width: 3px;
        }
        .box3-4{
            width: 300px;
            height: 40px;
            margin: 10px;
            padding: 10px;
            border-style: solid;
            border-width: 5px;
            border-color: #333;
        }
        .box4-1 {
            background-color: lightblue;
            width: 100px;
            height: 100px;
            margin: 30px;
        }
        .box4-2{
            background-color: lightgreen;
            width: 100px;
            height: 100px; 
            margin: 20px;
        }
        .box4-3{
            background-color: lightgreen;
            width: 100px;
            height: 100px; 
            margin: -40px;
        }
        .container {
            width: 400px;
            height: 300px;
            background-color: #eee;
        }
        .box4-4 {
            width: 100px;
            height: 100px;
            margin-top: 10%;
            background-color: orange;
        }
        .solid   { border-style: solid; }
        .dashed  { border-style: dashed; }
        .dotted  { border-style: dotted; }
        .double  { border-style: double; }
        .groove  { border-style: groove; }
        .ridge   { border-style: ridge; }
        .inset   { border-style: inset; }
        .outset  { border-style: outset; }
        .none    { border-style: none; border-color: red; background-color: #f9f9f9; }
        .hidden  { border-style: hidden; border-color: red; background-color: #f9f9f9; }
        .width1  { border-width: 5px 10px 15px 20px; }
        .width2  { border-width: 2px 4px;}
        .width2  { border-width: 2px 10px 5px;}
        .color1  { border-color: red green; }
        .color2  { border-color: red green blue; }
        .color3  { border-color: red green blue yellow;}
        .radius1 { border-radius: 5px; }
        .radius2 { border-radius: 5px 10px; }
        .radius3 { border-radius: 5px 10px 15px; }

        button {
            padding: 10px 20px;
            font-size: 16px;
            border: 2px solid #ccc;
            background-color: white;
            cursor: pointer;
        }

        /* 聚焦时显示轮廓 */
        button:hover {
        outline: 3px dashed blue;
        outline-offset: 20px;
        }
    </style>
</head>
<body>
    <h2>一.盒子模型基础概念</h2>
    <div class="box1">
        <p>盒子模型</p>
    </div>
    <h2>二.盒子模型的两种模式</h2>
    <div class="box2-1">
        <p>content-box模式</p>
    </div>
    <div class="box2-2">
        <p>border-box模式</p>
    </div>
    <h2>三.Border边框</h2>
    <h3>1.边框样式</h3>
    <div class="box3-1 solid">solid（实线）</div>
    <div class="box3-1 dashed">dashed（虚线）</div>
    <div class="box3-1 dotted">dotted（点状线）</div>
    <div class="box3-1 double">double（双实线）</div>
    <div class="box3-1 groove">groove（雕刻凹槽）</div>
    <div class="box3-1 ridge">ridge（凸起边框）</div>
    <div class="box3-1 inset">inset（内凹边框）</div>
    <div class="box3-1 outset">outset（外凸边框）</div>
    <div class="box3-1 none">none（无边框）</div>
    <div class="box3-1 hidden">hidden（隐藏边框）</div>
    <h3>2.边框宽度</h3>
    <div class="box3-2 width1">border-width: 5px 10px 15px 20px; </div>
    <div class="box3-2 width2">border-width: 2px 10px 5px;</div>
    <div class="box3-2 width2">border-width: 2px 10px 5px;</div>
    <h3>3.边框颜色</h3>
    <div class="box3-3 color1">border-color: red;</div>
    <div class="box3-3 color2">border-color: red green blue;</div>
    <div class="box3-3 color3">border-color: red green blue yellow;</div>
    <h3>4.边框圆角</h3>
    <div class="box3-4 radius1">border-radius: 5px;</div>
    <div class="box3-4 radius2">border-radius: 5px 10px;</div>
    <div class="box3-4 radius3">border-radius: 5px 10px 15px;</div>
    
    <h2>四.outline轮廓</h2>
    <p>鼠标悬停时显示 Outline</p>
    <button>把鼠标移上来试试</button>

    <h2>五.margin外边距</h2>
    <h3>1.外边距合并：两个正值示例</h3>
    <div class="box4-1">margin: 30px;</div>
    <div class="box4-2">margin: 20px;</div>
    <br>
    <h3>2.外边距合并：一正一负示例</h3>
    <div class="box4-1">margin: 30px;</div>
    <div class="box4-3">margin: -40px;</div>
    <h3>3.百分值示例</h3>
    <div class="container">
    <div class="box4-4"></div>
  </div>
    
</body>
</html>
```