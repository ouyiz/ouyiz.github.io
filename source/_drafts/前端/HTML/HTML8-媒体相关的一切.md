# 一.本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>HTML媒体示范页面</title>
</head>
<body>
    <h1>多媒体示范页面</h1>
    <!-- 音频播放 -->
    <h2>示例音频</h2>
    <audio controls>
        <source src="https://www.w3schools.com/html/horse.mp3" type="audio/mpeg">
        <source src="https://www.w3schools.com/html/horse.ogg" type="audio/ogg">
        您的浏览器不支持音频播放。
    </audio>

    <!-- 视频播放 -->
    <h2>示例视频</h2>
    <video controls width="600" poster="https://via.placeholder.com/600x300.png?text=视频封面">
        <source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4">
        <source src="https://www.w3schools.com/html/mov_bbb.ogg" type="video/ogg">
        您的浏览器不支持视频播放。
    </video>


    <!-- 外部媒体嵌入 -->
    <h2>嵌入B站视频示例</h2>
    <iframe src="https://player.bilibili.com/player.html?bvid=BV1Q5411d7uB&autoplay=0"
            width="800"
            height="500"
            frameborder="0"
            allowfullscreen>
    </iframe>

    <!-- 外部地图嵌入示例 -->
    <h2>嵌入百度地图示例</h2>
    <iframe src="https://api.map.baidu.com/lbsapi/getpoint/index.html"
            width="800"
            height="500"
            frameborder="0">
    </iframe>

    <!-- SVG图形示例 -->
    <h2>SVG图形示例</h2>
    <svg width="100" height="100">
        <circle cx="50" cy="50" r="40" stroke="black" fill="red" />
    </svg>

    <!-- Canvas 绘制矩形 -->
    <h2>Canvas 绘制矩形</h2>
    <canvas id="myCanvas" width="400" height="300" style="border:1px solid #000;">
        您的浏览器不支持 Canvas，请升级！
    </canvas>
    <script>
        // 获取canvas对象
        var canvas = document.getElementById("myCanvas");
        var ctx = canvas.getContext("2d");  // 获取2D绘图环境
        // 设置填充颜色
        ctx.fillStyle = "red";
        // 绘制矩形：x=50, y=50，宽100，高80
        ctx.fillRect(50, 50, 100, 80);
    </script>
</body>

</html>
```

## 二、音频元素 `<audio>`

插入音频文件。

```html
<audio src="music.mp3" controls></audio>
```

或推荐写法：

```html
<audio controls>
  <source src="music.mp3" type="audio/mpeg">
  <source src="music.ogg" type="audio/ogg">
  浏览器不支持音频播放！
</audio>
```
- 浏览器解析 `<audio>`，显示播放按钮；  
- 自动尝试播放 `music.mp3`，如果不支持，再试 `music.ogg`；  
- 如果都不支持，就显示“浏览器不支持音频播放！”的文字提示。

| 属性         | 作用                  |
| ---------- | ------------------- |
| `src`      | 音频文件路径              |
| `controls` | 显示播放器控制条（播放/暂停/音量等） |
| `autoplay` | 自动播放音频              |
| `loop`     | 循环播放                |
| `muted`    | 静音播放                |


## 三、视频元素 `<video>`

插入视频文件。
```html
<video src="video.mp4" controls width="640" height="360"></video>
```

推荐多格式兼容写法：
```html
<video controls width="640" height="360">
  <source src="video.mp4" type="video/mp4">
  <source src="video.ogg" type="video/ogg">
  浏览器不支持视频播放！
</video>
```

|属性|作用|
|---|---|
|`src`|视频文件路径|
|`controls`|显示播放器控制条|
|`autoplay`|自动播放|
|`loop`|循环播放|
|`muted`|静音播放|
|`poster`|视频封面图（播放前显示）|


## 四、嵌入外部媒体：`<iframe>`
用来嵌入网页、视频等外部资源。
```html
<iframe src="https://www.bilibili.com" width="800" height="600"></iframe>
```

常用于：
- 视频网站嵌入（如 YouTube、B站）
- 地图嵌入（如百度地图、Google地图）
- 嵌入网页片段。

|属性|作用|
|---|---|
|`src`|嵌入内容的地址|
|`width`|宽度（像素/百分比）|
|`height`|高度（像素/百分比）|
|`frameborder`|是否显示边框（已过时，CSS更常用）|


## 五、媒体补充：SVG 和 Canvas

| 标签         | 用途                          |
| ---------- | --------------------------- |
| `<svg>`    | 矢量图形，适合绘制图标、图表，放大不失真。       |
| `<canvas>` | 画布，配合 JavaScript 绘制动态图形/游戏。 |

示例（SVG）：
```html
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black" fill="red" />
</svg>
```

示例（canvs）:
```HTML
    <!-- Canvas 绘制矩形 -->
    <h2>Canvas 绘制矩形</h2>
    <canvas id="myCanvas" width="400" height="300" style="border:1px solid #000;">
        您的浏览器不支持 Canvas，请升级！
    </canvas>
    <script>
        // 获取canvas对象
        var canvas = document.getElementById("myCanvas");
        var ctx = canvas.getContext("2d");  // 获取2D绘图环境
        // 设置填充颜色
        ctx.fillStyle = "red";
        // 绘制矩形：x=50, y=50，宽100，高80
        ctx.fillRect(50, 50, 100, 80);
    </script>
```