## 一. 渐变概述
- 渐变是CSS中通过平滑过渡多种颜色来创建渐变效果的技术，常用于背景色或边框色等。
- 渐变不是图片，是CSS的函数，动态渲染，支持响应式。

## 二. 渐变的基本函数类型

|类型|描述|示例|
|---|---|---|
|`linear-gradient()`|线性渐变，颜色沿直线渐变|`linear-gradient(to right, red, blue)`|
|`radial-gradient()`|径向渐变，颜色从中心向外扩散|`radial-gradient(circle, red, blue)`|
|`conic-gradient()`|锥形渐变，颜色围绕中心点旋转|`conic-gradient(from 0deg, red, blue)`|
## 三. `linear-gradient()` 线性渐变
- 语法：
```css
linear-gradient([角度或方向], 颜色停止点1, 颜色停止点2, ...)
```
- 参数：
    - 关键字方向
	    - `to top`：从下到上渐变（颜色从下方开始，向上）
	    - `to bottom`：从上到下渐变（颜色从上方开始，向下）
	    - `to left`：从右到左渐变
	    - `to right`：从左到右渐变（默认方向）
	    - `to top right`：从左下角到右上角渐变
	    - `to bottom left`：从右上角到左下角渐变
    - 角度：如 `45deg`, `90deg` 等（相对于水平向右的方向）
- 颜色停止点（stop）：定义渐变颜色及其位置，可以是百分比或长度。
	- `颜色 [位置]`

示例：
```css
linear-gradient(45deg, red, yellow 50%, green)
```
- 红色在起点（默认是0%）。
- 黄色在50%位置。
- 绿色在终点（默认是100%）。

## 四. `radial-gradient()` 径向渐变
- `radial-gradient()` 用于创建从某一点向四周发散的颜色渐变，类似水波纹或光晕的效果。颜色从中心向外扩散。
- 语法：
```css
radial-gradient([形状大小位置], 颜色停止点1, 颜色停止点2, ...)
```
- 参数：
    - 形状：`circle`（圆形）或 `ellipse`（椭圆，默认）
    - 大小：决定渐变范围的大小
	    - `closest-side`，渐变半径为从中心点到最近边的距离
	    - `farthest-side`，渐变半径为从中心点到最近边的距离
	    - `closest-corner`，渐变半径为从中心点到最近角落的距离
	    - `farthest-corner`，渐变半径为从中心点到最远角落的距离（默认值）
    - 位置：决定渐变的起始中心点
	    - `at center`：中心（默认）
	    - `at top left`：左上角
	    - `at bottom right`：右下角
	    - `at 25% 75%`：自定义百分比坐标（横 25%、纵 75%）

示例：
```css
background: radial-gradient(circle farthest-corner at top left, red, blue);
```
- 形状是 `circle`（圆形）；
- 大小为 `farthest-corner`，即从左上角向最远角（右下角）扩展；
- 渐变从左上角的红色变为右下角的蓝色。

## 五. `conic-gradient()` 锥形渐变
- `conic-gradient()` 是 CSS 中用于创建围绕中心点旋转的颜色渐变。以圆心为中心，顺时针沿角度分布颜色，
- 语法：
```css
conic-gradient([起始角度] at [位置], 颜色停止点1, 颜色停止点2, ...)
```
- 参数：
	- 起始角度：`from <angle>`
		- 表示颜色开始旋转的起点角度；
		- 单位可以是 `deg`, `rad`, `grad`, `turn`；
		- `0deg` 表示正上方，顺时针方向递增。
	- 位置：`at <position>`
		- 指定渐变中心点的位置；
		- `at center`：中心（默认）
	    - `at top left`：左上角
	    - `at bottom right`：右下角
	    - `at 25% 75%`：自定义百分比坐标（横 25%、纵 75%）
	- 颜色停止点：每个停止点指定颜色及其扇形位置范围
		- 写法类似于线性/径向渐变；

示例：
```css
background: conic-gradient(from 0deg at center, red 0deg 90deg, green 90deg 270deg, blue 270deg 360deg);
```
- 创建了一个以元素中心为起点的三色锥形渐变背景
- 红色占 0° ~ 90°
- 绿色占 90° ~ 270°
- 蓝色占 270° ~ 360°

## 六. 透明渐变（带透明度）
- 可用 `rgba()` 或 `hsla()` 设置透明渐变。
- 透明度的变化可实现渐隐效果。

示例：
```css
background: linear-gradient(to bottom, rgba(255,0,0,1), rgba(255,0,0,0));
```
线性渐变背景，颜色从 完全不透明的红色（上方）渐变到 完全透明的红色（下方），呈现出 红色逐渐消失 的视觉效果。

## 七.本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <title>CSS 渐变全覆盖示例（美观配色）</title>
  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", sans-serif;
      background: #f7f9fb;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 20px;
      padding: 20px;
    }

    .box {
      height: 200px;
      border-radius: 12px;
      color: #333;
      font-size: 18px;
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
      text-align: center;
      padding: 1em;
      background-size: 100% 100%;
    }

    .linear-angle {
      background: linear-gradient(45deg, #a1c4fd, #c2e9fb);
    }

    .linear-direction {
      background: linear-gradient(to right, #fbc2eb, #a6c1ee);
    }

    .radial-default {
      background: radial-gradient(ellipse farthest-corner at top left, #fddb92, #d1fdff);
    }

    .radial-circle {
      background: radial-gradient(circle closest-side at 25% 75%, #b8cbb8, #e2c58b);
    }

    .conic-angles {
      background: conic-gradient(
        from 0deg at center,
        #fad0c4 0deg 90deg,
        #ffd1ff 90deg 270deg,
        #c2ffd8 270deg 360deg
      );
    }

    .conic-smooth {
      background: conic-gradient(
        from 45deg at bottom right,
        #ffecd2,
        #fcb69f,
        #a1c4fd,
        #d4fc79,
        #96e6a1,
        #ffecd2
      );
    }

    .transparent-gradient {
      background: linear-gradient(to bottom, rgba(173,216,230,1), rgba(173,216,230,0));
      color: #333;
    }
  </style>
</head>
<body>

  <div class="box linear-angle">线性渐变（角度 + 停止点）</div>
  <div class="box linear-direction">线性渐变（方向 to right）</div>
  <div class="box radial-default">径向渐变（椭圆，左上角）</div>
  <div class="box radial-circle">径向渐变（圆形，自定义位置）</div>
  <div class="box conic-angles">锥形渐变（角度控制范围）</div>
  <div class="box conic-smooth">锥形渐变（彩虹平滑渐变）</div>
  <div class="box transparent-gradient">透明渐变（淡蓝 → 透明）</div>

</body>
</html>

```


