## 一. `display` —— 控制元素的显示类型

**作用**：定义元素在页面中以哪种盒模型展现，是网页布局的基础。可控制元素是块级、内联、弹性布局、网格布局等形式，或完全从布局中移除。  
**语法**：
```css
selector {
    display: 类型;
}
```

| 属性值            | 说明                                    |
| -------------- | ------------------------------------- |
| `none`         | 元素完全隐藏，并且不占据任何页面空间。常用于“显示/隐藏”功能实现。    |
| `block`        | 元素作为块级元素显示，占据整行，可设置宽高、margin/padding。 |
| `inline`       | 元素作为内联元素显示，占据内容宽度，无法设置宽高。             |
| `inline-block` | 元素表现为内联元素，但可以设置宽高。                    |
| `flex`         | 启用弹性盒布局模型，方便实现响应式排列。子元素变成“弹性子项”。      |
| `inline-flex`  | 与 `flex` 相似，但表现为内联元素。                 |
| `grid`         | 启用 CSS 网格布局。可将子项排列在定义好的网格区域中。         |
| `inline-grid`  | 与 `grid` 相似，但元素为内联行为。                 |
| `table`        | 元素表现得像 `<table>`，可用于构造自定义表格结构。        |
| `table-row`    | 表现为 `<tr>` 行。                         |
| `table-cell`   | 表现为 `<td>` 单元格。                       |
| `list-item`    | 元素表现为列表项（如 `<li>`），自动带有项目符号。          |
| `flow-root`    | 创建一个独立的块级格式化上下文，用于清除内部浮动影响。           |
| `contents`     | 不渲染当前元素的盒子，仅渲染其子元素（注意兼容性问题）。          |
| `inherit`      | 继承父元素的 `display` 属性值。                 |
| `initial`      | 设置为默认值（大多数元素默认为 `inline`）。            |
| `unset`        | 如果父元素定义了该属性则继承，否则设置为默认值。              |

## 二. block ,Inline与inline-block

**1.Block、Inline 和 Inline-block 的关系与区别**

| 类型               | 是否独占一行 | 宽高控制    | 典型标签（默认）                         | 应用场景示例              |
| ---------------- | ------ | ------- | -------------------------------- | ------------------- |
| **Block**        | 是      | 可以设置宽高  | `<div>`, `<p>`, `<h1>` 等         | 大块结构布局，容器，段落等       |
| **Inline**       | 否      | 不支持宽高设置 | `<span>`, `<a>`, `<strong>`      | 文本修饰，如粗体、链接、关键字高亮   |
| **Inline-block** | 否      | 支持宽高设置  | 通过 CSS 设置 `display:inline-block` | 需要排成一行的块级元素，如按钮、菜单项 |
**2.`inline-block`**
 - `display: inline-block` 元素默认的 **垂直对齐方式** 是 `baseline`，这意味着多个` inline-block` 元素会以其文本的基线（baseline）对齐。解决方法为使用`vertical-align: top;`

### 三. flex与inline-flex

## 1.`display: flex` —— 弹性盒容器（块级）
**作用**：将元素设为弹性容器，使其子元素在一个主轴上排列并具有灵活的布局能力。
**语法**：
```css
.container {
    display: flex;
}
```
**说明**：
- 容器变成块级元素，独占一行。
- 子元素（称为“弹性项目”）会按主轴排列，自动适配空间，支持自动换行、对齐等。

### 2.`display: inline-flex` —— 弹性盒容器（内联）
**作用**：与 `flex` 相同，只是容器行为变成内联元素，不会独占整行。
**语法**：
```css
.inline-container {
    display: inline-flex;
}
```
**说明**：
- 元素表现得像 `inline`（不会强制换行），但其内部子元素使用弹性布局。
- 常用于按钮组、图标+文字等紧凑排布场景。

### 3.属性
- **主轴（main axis）**：默认是水平方向，决定子元素如何横向排列。
- **交叉轴（cross axis）**：与主轴垂直，默认是竖直方向。
- **容器**：设置了 `display: flex` 或 `inline-flex` 的元素。
- **项目（item）**：容器内的子元素。
**1）`flex-direction` —— 设置主轴方向**
**作用**：决定子元素沿哪个方向排列。
**语法**：
```css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

|属性值|说明|
|---|---|
|`row`|默认，从左到右横向排列|
|`row-reverse`|从右到左横向排列|
|`column`|从上到下纵向排列|
|`column-reverse`|从下到上纵向排列|

**示例**：
```css
.container {
  display: flex;
  flex-direction: column;
}
```

**2）`justify-content` —— 主轴上的对齐方式**
**作用**：控制子元素在主轴上的排列和间距。

|属性值|说明|
|---|---|
|`flex-start`|靠主轴起点对齐（默认）|
|`flex-end`|靠主轴终点对齐|
|`center`|居中对齐|
|`space-between`|两端对齐，项目之间平均分配间距|
|`space-around`|项目两边间距相等|
|`space-evenly`|所有间距完全一致|

**示例**：
```css
.container {
  display: flex;
  justify-content: space-between;
}
```

**3） `align-items` —— 交叉轴上的对齐方式（单行）**
**作用**：控制子元素在垂直方向（默认）上的对齐方式。

|属性值|说明|
|---|---|
|`stretch`|默认，高度自动拉伸填满容器|
|`flex-start`|顶部对齐|
|`flex-end`|底部对齐|
|`center`|垂直居中|
|`baseline`|按文字基线对齐，适用于文字或表单元素对齐|

**示例**：
```css
.container {
  display: flex;
  align-items: center;
}
```

**4）`flex-wrap` —— 是否换行**
**作用**：控制子元素在空间不足时是否自动换行。

|属性值|说明|
|---|---|
|`nowrap`|默认，不换行，可能挤压|
|`wrap`|允许换行|
|`wrap-reverse`|允许换行，换行方向相反|

**示例**：
```css
.container {
  display: flex;
  flex-wrap: wrap;
}
```

**5）`align-content` —— 多行的交叉轴对齐**
**作用**：在**多行**布局时，控制各行整体在交叉轴上的分布方式。

|属性值|说明|
|---|---|
|`flex-start`|所有行顶部对齐|
|`flex-end`|所有行底部对齐|
|`center`|多行垂直居中|
|`space-between`|行与行之间等距，两端贴边|
|`space-around`|每行上下间距相等|
|`space-evenly`|所有行的间距完全一致|
|`stretch`|拉伸行高，填满容器（默认）|

**示例**：
```css
.container {
  display: flex;
  flex-wrap: wrap;
  align-content: center;
}
```

**6）`gap` —— 项目之间的间距**
**作用**：直接设置项目之间的间距（支持行间和列间距），比用 `margin` 更清爽。
**语法**：
```css
.container {
  display: flex;
  gap: 20px;               /* 横纵一致 */
  row-gap: 10px;           /* 行间距 */
  column-gap: 30px;        /* 列间距 */
}
```

### 4.子项属性
**常用 Flex 子项属性（五个）**

|属性名|应用对象|用途描述|
|---|---|---|
|`flex-grow`|子项|剩余空间如何按比例分配给子项|
|`flex-shrink`|子项|空间不足时如何按比例缩小子项|
|`flex-basis`|子项|项目的初始尺寸（未考虑剩余空间）|
|`flex`|子项|`grow + shrink + basis` 简写|
|`align-self`|子项|控制单个项目在交叉轴上的对齐方式|
**1）`flex-grow` —— 放大比例**
**作用**：定义项目在**容器有剩余空间**时的放大比例。
**语法**：
```css
.item {
  flex-grow: 1;
}
```

|值|说明|
|---|---|
|`0`|默认值，不放大|
|`>0`|表示放大的权重，越大分到的空间越多|

**2） `flex-shrink` —— 缩小比例**
**作用**：当**容器空间不足**时，项目缩小的比例。
**语法**：
```css
.item {
  flex-shrink: 1;
}
```

|值|说明|
|---|---|
|`0`|项目不会被缩小（可能超出容器）|
|`>0`|表示缩小比例，越大越容易被压缩|

**3）`flex-basis` —— 初始大小**
**作用**：项目在分配多余空间前的**基础大小**。
**语法**：
```css
.item {
  flex-basis: 200px;
}
```

|值|说明|
|---|---|
|`auto`|默认，使用内容或 `width/height` 决定|
|具体数值|指定初始宽度或高度，取代 `width` 属性|
**注意**：
- `flex-basis` 会覆盖 `width`（当方向为 row）
- `flex-basis` 会覆盖 `height`（当方向为 column）

**4）`flex` —— 综合简写**
**语法**：
```css
.item {
  flex: <grow> <shrink> <basis>;
}
```

**常用形式**：

|写法|相当于|
|---|---|
|`flex: 1`|`flex-grow: 1; flex-shrink: 1; flex-basis: 0%`|
|`flex: 0 1 auto`|默认值|
|`flex: 2 2 100px`|放大2倍，缩小2倍，初始大小100px|

**5） `align-self` —— 单个项目的对齐方式**
**作用**：覆盖容器的 `align-items`，只作用于当前项目。
**语法**：
```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

|值|说明|
|---|---|
|`auto`|默认，继承容器的 `align-items`|
|`flex-start`|顶部对齐|
|`flex-end`|底部对齐|
|`center`|垂直居中|
|`baseline`|与文字基线对齐|
|`stretch`|自动拉伸（默认）|
