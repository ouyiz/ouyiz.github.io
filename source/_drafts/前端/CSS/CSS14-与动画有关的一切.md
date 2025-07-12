## 一. 动画基本概述
- CSS 动画允许元素在一段时间内逐渐改变样式。
- 使用 `@keyframes` 定义动画关键帧，通过 `animation` 系列属性控制播放方式。

## 二. 动画的基本属性

| 属性                          | 描述                |
| --------------------------- | ----------------- |
| `animation-name`            | 指定动画的名称           |
| `animation-duration`        | 动画持续时间            |
| `animation-timing-function` | 动画的速度曲线           |
| `animation-delay`           | 动画延迟开始的时间         |
| `animation-iteration-count` | 动画循环次数            |
| `animation-direction`       | 动画播放方向            |
| `animation-fill-mode`       | 控制动画开始/结束后元素保持的状态 |
| `animation-play-state`      | 控制动画运行/暂停         |
可以用简写属性：
```css
animation: 动画名称 动画时长 缓动函数 延迟时间 重复次数 播放方向 填充模式;
animation: name duration timing-function delay iteration-count direction fill-mode play-state;
```

## 三. `@keyframes` 动画关键帧
`@keyframes` 是 CSS 动画的核心部分，它用来定义一个动画在不同时刻（关键帧）应该呈现的样式状态。你可以将它理解为“动画的剧本”。
每个关键帧可以指定一个动画进行到某一百分比时元素应该具备的样式。浏览器会在关键帧之间自动插值，实现平滑过渡。
```css
@keyframes example {
  0%   { transform: translateX(0); }
  50%  { transform: translateX(100px); }
  100% { transform: translateX(0); }
}
.box {
  animation: example 2s infinite;
}
```


## 四. `animation-name`
- **作用**：指定要绑定的动画名称（需对应 `@keyframes` 中定义的名称）
- **取值**：
    - 自定义名称（例如：`fadeIn`, `slide`）
    - `none`（默认值，表示无动画）
- **示例**：
    ```css
    animation-name: fadeIn;
    ```

## 五. `animation-duration`
- **作用**：设置动画播放一次所需的时间
- **取值**：
    - 时间值：如 `2s`（2 秒）、`500ms`（500 毫秒）
- **默认值**：`0s`（即不播放动画）
- **示例**：
    ```css
    animation-duration: 1.5s;
    ```
## 六. `animation-timing-function`
- **作用**：控制动画的速度曲线（即播放节奏）
- **常用取值**：

| 值                              | 描述          |
| ------------------------------ | ----------- |
| `linear`                       | 匀速（恒定速度）    |
| `ease`                         | 默认值，先慢后快再慢  |
| `ease-in`                      | 慢速开始        |
| `ease-out`                     | 慢速结束        |
| `ease-in-out`                  | 慢-快-慢       |
| `steps(n, start/end)`          | 分步动画（如打字效果） |
| `cubic-bezier(x1, y1, x2, y2)` | 自定义贝塞尔曲线    |

- **示例**：
    ```css
    animation-timing-function: ease-in-out;
    animation-timing-function: steps(5, end);
    ```

## 七. `animation-delay`
- **作用**：设置动画开始前的延迟时间
- **取值**：
    - 时间值：如 `1s`, `500ms`
    - 可为负值：表示动画从中间某个时间点开始播放
- **默认值**：`0s`
- **示例**：
    ```css
    animation-delay: 0.5s;
    ```

## 八. `animation-iteration-count`
- **作用**：设置动画的播放次数
- **取值**：
    - 数字（如 `1`, `3`）
    - `infinite`（无限循环）
- **默认值**：`1`
- **示例**：
    ```css
    animation-iteration-count: infinite;
    ```

## 九. `animation-direction`
- **作用**：设置每次循环动画的播放方向
- **取值**：

|值|描述|
|---|---|
|`normal`|每次都按正向播放（默认）|
|`reverse`|每次都反向播放|
|`alternate`|正向、反向交替播放|
|`alternate-reverse`|反向、正向交替播放（从反向开始）|

- **示例**：
    ```css
    animation-direction: alternate;
    ```

## 十. `animation-fill-mode`
- **作用**：定义动画在开始前和结束后元素的状态
- **取值**：

| 值           | 描述                            |
| ----------- | ----------------------------- |
| `none`      | 默认值，不保留动画状态                   |
| `forwards`  | 动画结束后保留结束状态                   |
| `backwards` | 动画开始前应用起始状态                   |
| `both`      | 同时应用 `backwards` 和 `forwards` |

- **示例**：
    ```css
    animation-fill-mode: both;
    ```
    
## 十一. `animation-play-state`
- **作用**：控制动画是“播放”还是“暂停”
- **取值**：
    - `running`：正在播放（默认值）
    - `paused`：暂停
- **示例**：
    ```css
    animation-play-state: paused;
    ```
- **常用于交互控制**：
    ```css
    .box:hover {
      animation-play-state: paused;
    }
    ```

## 十二.本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <title>CSS 动画多例子演示</title>
  <style>
    body {
      font-family: "Segoe UI", sans-serif;
      padding: 20px;
      background: #f5f5f5;
    }

    h1 {
      margin-bottom: 10px;
    }

    .section {
      margin-bottom: 40px;
    }

    .label {
      margin: 10px 0 5px;
      font-weight: bold;
    }

    .tooltip {
      background: #eef;
      padding: 10px;
      border-left: 5px solid #339;
      margin-bottom: 10px;
      font-size: 14px;
    }

    .demo-box {
      width: 100px;
      height: 100px;
      margin: 10px 0;
      background: tomato;
      border-radius: 8px;
    }

    .move {
      animation: moveRight 2s linear infinite alternate;
    }

    .fade-in {
      animation: fadeIn 2s ease-in-out 1 forwards;
    }

    .scale {
      animation: scaleUp 1s ease-in-out infinite alternate;
    }

    .rotate {
      animation: rotate360 3s linear infinite;
    }

    .steps {
      font-family: monospace;
      width: 12ch;
      overflow: hidden;
      white-space: nowrap;
      border-right: 2px solid;
      animation: typing 4s steps(12, end) 1 forwards;
    }

    .paused:hover {
      animation-play-state: paused;
    }

    /* Keyframes 定义 */
    @keyframes moveRight {
      from { transform: translateX(0); }
      to { transform: translateX(150px); }
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    @keyframes scaleUp {
      0% { transform: scale(1); }
      100% { transform: scale(1.5); }
    }

    @keyframes rotate360 {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }

    @keyframes typing {
      from { width: 0; }
      to { width: 12ch; }
    }
  </style>
</head>
<body>

  <h1>CSS 动画多个示例演示</h1>

  <!-- 位移动画 -->
  <div class="section">
    <div class="tooltip">🔁 使用 <code>alternate</code> 方向来让元素左右来回移动</div>
    <div class="demo-box move"></div>
    <div class="label">animation: moveRight 2s linear infinite alternate;</div>
  </div>

  <!-- 淡入动画 -->
  <div class="section">
    <div class="tooltip">🌟 元素加载时逐渐淡入，使用 <code>animation-fill-mode: forwards</code> 保持最终状态</div>
    <div class="demo-box fade-in"></div>
    <div class="label">animation: fadeIn 2s ease-in-out 1 forwards;</div>
  </div>

  <!-- 缩放动画 -->
  <div class="section">
    <div class="tooltip">🔍 元素不断放大缩小</div>
    <div class="demo-box scale"></div>
    <div class="label">animation: scaleUp 1s ease-in-out infinite alternate;</div>
  </div>

  <!-- 旋转动画 -->
  <div class="section">
    <div class="tooltip">🔄 元素连续旋转 360 度</div>
    <div class="demo-box rotate"></div>
    <div class="label">animation: rotate360 3s linear infinite;</div>
  </div>

  <!-- 步进动画 -->
  <div class="section">
    <div class="tooltip">⌨️ 使用 <code>steps()</code> 模拟打字机效果</div>
    <div class="steps">Hello, world!</div>
    <div class="label">animation: typing 4s steps(12, end) 1 forwards;</div>
  </div>

  <!-- 鼠标暂停动画 -->
  <div class="section">
    <div class="tooltip">⏸️ 鼠标悬停时动画暂停（使用 <code>animation-play-state: paused</code>）</div>
    <div class="demo-box move paused"></div>
    <div class="label">animation-play-state: paused（悬停生效）</div>
  </div>

</body>
</html>

```