CSS 多列布局允许你像报纸或杂志那样将内容分为多个列。你可以控制列数、列宽、列间距、列边框等。
## 一.常用属性总览

|属性|说明|
|---|---|
|`columns`|简写属性，设置列数和列宽|
|`column-count`|指定列数|
|`column-width`|指定每列的宽度|
|`column-gap`|指定列与列之间的间距|
|`column-rule`|简写属性，设置列之间的分隔线|
|`column-rule-width`|分隔线宽度|
|`column-rule-style`|分隔线样式（如 solid、dashed）|
|`column-rule-color`|分隔线颜色|
|`column-span`|指定元素跨越多少列|
|`column-fill`|控制列如何填充内容|

## 二. `column-count`
- **功能**：指定列数（整数）
- **取值范围**：
    - 正整数（如 `1`, `2`, `3`, ...）
    - `auto`（默认，浏览器自动决定列数）
**示例**：
```css
column-count: 3;  /* 分为3列 */
column-count: auto; /* 浏览器自动决定 */
```

## 三. `column-width`
- **功能**：指定每列的理想宽度
- **取值范围**：
    - 长度单位（如 px, em, rem, %, vw 等）
    - `auto`（默认值，表示自动计算列宽）
**示例**：
```css
column-width: 200px; /* 每列理想宽度200像素 */
column-width: 30vw;  /* 每列宽度为视口宽度30% */
column-width: auto;  /* 自动 */
```


## 四. `columns`（简写属性）
- **功能**：同时设置列宽和列数
- **取值格式**：
    - `<column-width> <column-count>`
    - 例如：`columns: 200px 3;`
    - 也可以只写一个值，代表列数或列宽
**示例**：
```css
columns: 200px 3;  /* 列宽200px，列数3 */
columns: 3;        /* 只指定列数3 */
columns: 150px;    /* 只指定列宽150px */
```

---

## 五. `column-gap`
- **功能**：设置列之间的间距
- **取值范围**：
    - 长度单位（px, em, rem, %, vw, vh等）
    - `normal`（默认，一般相当于 `1em`）
**示例**：
```css
column-gap: 20px;  /* 列间距20像素 */
column-gap: 1.5em; /* 列间距1.5倍字体大小 */
column-gap: normal; /* 浏览器默认间距 */
```

## 六. `column-rule`（简写）
- **功能**：设置列之间分隔线，包含宽度、样式、颜色
- **组成**：
    - `column-rule-width`：宽度，长度单位或关键词
    - `column-rule-style`：边框样式，支持的值见下
    - `column-rule-color`：颜色
**`column-rule-style` 可选值：**

|值|说明|
|---|---|
|`none`|无分隔线（默认）|
|`solid`|实线|
|`dashed`|虚线|
|`dotted`|点线|
|`double`|双线|
|`groove`|3D凹槽效果|
|`ridge`|3D脊状效果|
|`inset`|3D内嵌效果|
|`outset`|3D外突效果|

**示例**：
```css
column-rule: 2px solid black;
column-rule: 1px dashed gray;
column-rule: thin dotted red; /* 宽度关键字：thin, medium, thick */
```

## 七. `column-span`
- **功能**：控制元素是否跨越所有列
- **取值**：
    - `none`（默认）：元素不跨列
    - `all`：元素横跨所有列
**示例**：
```css
h2 {
  column-span: all;
}
```
<img src="file-20250523154543985.png" style="width: 400px; height: auto;">
## 八. `column-fill`
- **功能**：控制内容如何填充多列，主要对有固定高度的容器有效
- **取值**：

| 值         | 说明                  |
| --------- | ------------------- |
| `balance` | 内容在列间平均分配（默认）       |
| `auto`    | 一列填满后再填下一列，类似连续流式布局 |
**示例**：
```css
.container {
  column-fill: balance;
}

.container-alt {
  column-fill: auto;
}
```

## 九.本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>CSS 多列布局完整示例</title>
<style>
  body {
    font-family: "微软雅黑", Arial, sans-serif;
    padding: 20px;
  }

  /* 容器设置多列，示范columns简写 */
  .container {
    /* 设定理想列宽200px，最多3列 */
    columns: 200px 3;
    /* 列间距 */
    column-gap: 20px;
    /* 列间分隔线 */
    column-rule: 2px solid #007acc;
    /* 高度固定，配合column-fill演示 */
    height: 250px;
    /* 内容填充方式 */
    column-fill: balance; /* 默认，平均分配 */
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 40px;
  }

  /* 使用单独属性替代简写，演示更细粒度控制 */
  .container-alt {
    column-count: 4;           /* 4列 */
    column-width: 150px;       /* 每列理想宽150px */
    column-gap: 1.5em;         /* 列间距1.5倍字体大小 */
    column-rule-width: 3px;    /* 分隔线宽度3px */
    column-rule-style: dashed; /* 虚线 */
    column-rule-color: #ff5722;/* 橙红色 */
    column-fill: auto;         /* 自动填满一列再填下一列 */
    height: 250px;
    border: 1px solid #ccc;
    padding: 10px;
  }

  /* 标题跨列示例 */
  h2 {
    /* 跨越所有列 */
    column-span: all;
    background-color: #f0f8ff;
    padding: 6px 10px;
    margin-top: 0;
    border: 1px solid #007acc;
    color: #007acc;
  }

  /* 普通段落样式 */
  p {
    margin: 0 0 1em;
    line-height: 1.5;
  }
</style>
</head>
<body>

<h1>CSS 多列布局示例</h1>

<section class="container">
  <h2>多列布局 - columns 简写</h2>
  <p>这是一个使用 <code>columns: 200px 3;</code> 的容器，表示每列理想宽度 200px，最多 3 列。</p>
  <p>列间距为 20px，列间有实线分隔线，容器高度固定为 250px，内容在各列之间均匀分配（balance）。</p>
  <p>文本会自动分配到多列中，标题跨越所有列，显得醒目。</p>
  <p>你可以看到内容均匀分布，分隔线清晰，标题视觉突出。</p>
  <p>更多内容用来展示多列效果，看看如何自动排版。</p>
</section>

<section class="container-alt">
  <h2>多列布局 - 单独属性详细控制</h2>
  <p>这个容器使用单独属性设置多列，<code>column-count: 4;</code>，4列布局。</p>
  <p>每列宽度理想为150px，列间距为1.5em，分隔线为3px虚线，颜色橙红色。</p>
  <p>容器高度同样为250px，但使用 <code>column-fill: auto;</code>，表现为填满一列再填下一列。</p>
  <p>你可以注意到内容填充方式不同，列高可能不均匀，视觉上更像连贯的内容流。</p>
  <p>标题同样跨列，增强分隔感和结构感。</p>
</section>

</body>
</html>
```