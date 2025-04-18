# 1.`background-color` —— 背景颜色
**作用**：设置元素的背景色。
**语法**：
```css
selector {
    background-color: 颜色值;
}
```

**示例**：
```css
div {
    background-color: lightblue;
}
```


# 2.`background-image` —— 背景图片
**作用**：给元素设置背景图片。
**语法**：
```css
selector {
    background-image: url('图片路径');
}
```

**示例**：
```css
body {
    background-image: url('background.jpg');
}
```

⚠️ 如果不想使用背景图：
```css
background-image: none;
```



# 3.`background-repeat` —— 背景重复方式
**作用**：控制背景图片是否在元素内重复（平铺）显示。
**常用值**：

|值|含义|
|---|---|
|`repeat`|水平+垂直重复（默认）|
|`repeat-x`|水平重复，垂直不重复|
|`repeat-y`|垂直重复，水平方向不重复|
|`no-repeat`|不重复，只显示一张|

**示例**：
```css
div {
	background-image: url('background.jpg');
    background-repeat: no-repeat;
}
```

**repeat效果：**
<img src="file-20250416161526338.png" style="width: 200px; height: auto;">

**repeat-x：**
<img src="file-20250416161746630.png" style="width: 200px; height: auto;">

**repeat-y：**
<img src="file-20250416161826044.png" style="width: 200px; height: auto;">

**no-repeat：**
<img src="file-20250416161909351.png" style="width: 200px; height: auto;">


# 4️. `background-position` —— 背景位置

**作用**：控制背景图片在元素中的位置。

**语法**：
```css
selector {
    background-position: x-axis y-axis;
}
```

**常用写法**：
- **x-axis**：背景图像的水平位置，可以是：
    - **关键字**：`left`, `center`, `right`，分别表示背景图像的左、中、右位置。
    - **长度值**：如 `10px`, `20%`，表示相对于元素的左边缘的距离。
- **y-axis**：背景图像的垂直位置，可以是：
    - **关键字**：`top`, `center`, `bottom`，分别表示背景图像的上、中、下位置。
    - **长度值**：如 `10px`, `20%`，表示相对于元素的上边缘的距离。 

**示例**：
```css
div {
    background-position: center center;
}
```

**center center：**
<img src="file-20250416162426700.png" style="width: 200px; height: auto;">

**left top：**
<img src="file-20250416162518668.png" style="width: 200px; height: auto;">



# 5.`background-size` —— 背景图片尺寸

**作用**：控制背景图缩放的方式。

**常用值**：

|值|含义|
|---|---|
|`auto`|按原始尺寸显示（默认）|
|`cover`|拉伸背景图以完全覆盖容器，可能超出一部分裁剪|
|`contain`|拉伸背景图以完整显示在容器内，可能有留白|
|`宽度 高度`|自定义尺寸，例如 `100px 50px` 或 `100% auto`|

**示例**：
```css
div {
    background-size: cover;
}
```

**auto：**
<img src="file-20250416162913499.png" style="width: 200px; height: auto;">

**contain：**
<img src="file-20250416163041974.png" style="width: 200px; height: auto;">

**contain：**
<img src="file-20250416163105476.png" style="width: 200px; height: auto;">

**300px 300px：**
<img src="file-20250416163139487.png" style="width: 200px; height: auto;">


# 6️. `background-attachment` —— 背景滚动方式

**作用**：设置背景图片是否随着页面滚动。

**常用值**：

|值|含义|
|---|---|
|`scroll`|跟随页面一起滚动（默认）|
|`fixed`|背景固定，不随内容滚动（视差效果）|
|`local`|背景随元素内部滚动|

**示例**：

```css
body {
    background-attachment: fixed;
}
```

**scroll与fixed的示例：**
<img src="file-20250416164042988.png" style="width: 300px; height: auto;">

<img src="file-20250416164122901.png" style="width: 300px; height: auto;">



# 7️.`background-clip` —— 背景裁剪区域

**作用**：定义背景绘制的范围。

**常用值**：

| 值             | 描述                                 |
| ------------- | ---------------------------------- |
| `border-box`  | 从边框外沿到内容区域，背景会显示在边框下方              |
| `padding-box` | 从内边距的起点开始绘制，不会绘制到边框下方              |
| `content-box` | 只在内容区域绘制，padding 和 border 部分都不显示背景 |

**示例**：
```css
div {
    background-clip: padding-box;
}
```

**border-box：**
<img src="file-20250416164952966.png" style="width: 200px; height: auto;">

**padding-box：**
<img src="file-20250416165047156.png" style="width: 200px; height: auto;">

**content-box：**
<img src="file-20250416165258492.png" style="width: 200px; height: auto;">


# 8. `background-origin` —— 背景定位基准点

**作用**：决定`background-position`相对于哪个区域定位。

**常用值**：

| 值             | 定位基准         |
| ------------- | ------------ |
| `padding-box` | 以内边距边界定位（默认） |
| `border-box`  | 以边框边界定位      |
| `content-box` | 以内容区域定位      |

**示例：**
**border-box：**
<img src="file-20250416170314660.png" style="width: 200px; height: auto;">
**padding-box：**
<img src="file-20250416170722916.png" style="width: 200px; height: auto;">

**content-box：**
<img src="file-20250416170850868.png" style="width: 200px; height: auto;">


# 9️. `background` —— 背景属性的简写形式

可以把背景相关属性一起写：

**语法**：
```css
background: 背景色 背景图片 位置/大小 是否重复 滚动方式;
```

**示例**：
```css
div {
    background: url("bg.png") no-repeat center/cover fixed #f2f2f2;
}
```

等同于：
```css
background-image: url("bg.png");
background-repeat: no-repeat;
background-position: center;
background-size: cover;
background-attachment: fixed;
background-color: #f2f2f2;
```


# 10.本节代码
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Background Repeat and Position Demo</title>
    <style>
        /* 通用样式 */
        .box {
            width: 500px;
            height: 500px;
            border: 2px solid black;
            margin: 10px;
        }


        .box_repeat {
            width: 900px;
            height: 900px;
            border: 2px solid black;
            margin: 10px;
        }

        .box_attachment {
            width: 600px;
            height: 300px;
            border: 2px solid black;
            margin: 10px;
            overflow: auto; /* 让内容超出出现滚动条 */
            background-image: url('images/test.jpg');
            background-repeat: no-repeat;
            background-position: center center;
            background-size: cover;
        }

  

        /* ---------------- background-repeat 示例 ---------------- */

        /* repeat - 横纵都重复 */

        .repeat {
            background-image: url('images/test.jpg');
            background-repeat: repeat;
        }

  

        /* repeat-x - 只横向重复 */
        .repeat-x {
            background-image: url('images/test.jpg');
            background-repeat: repeat-x;
        }

  

        /* repeat-y - 只纵向重复 */
        .repeat-y {
            background-image: url('images/test.jpg');
            background-repeat: repeat-y;
        }

  

        /* no-repeat - 不重复 */
        .no-repeat {
            background-image: url('images/test.jpg');
            background-repeat: no-repeat;
        }

  

        /* ---------------- background-position 示例 ---------------- */


        /* 位置居中 */
        .position-center {
            background-image: url('images/test.jpg');
            background-repeat: no-repeat;
            background-position: center center; /* 背景图片水平和垂直居中 */
        }

  

        /* 位置左上角 */
        .position-left-top {
            background-image: url('images/test.jpg');
            background-repeat: no-repeat;
            background-position: left top; /* 背景图片左上角 */
        }

  

        /* 位置右下角 */
        .position-right-bottom {
            background-image: url('images/test.jpg');
            background-repeat: no-repeat;
            background-position: right bottom; /* 背景图片右下角 */
        }

  

        /* 位置右上角 */
        .position-right-top {
            background-image: url('images/test.jpg');
            background-repeat: no-repeat;
            background-position: right top; /* 背景图片右上角 */
        }

    </style>
</head>

<body>
    <h2>Background Repeat 示例</h2>
    <div class="box_repeat repeat">repeat</div>
    <div class="box_repeat repeat-x">repeat-x</div>
    <div class="box_repeat repeat-y">repeat-y</div>
    <div class="box_repeat no-repeat">no-repeat</div>
  

    <h2>Background Position 示例</h2>
    <div class="box position-center">center center</div>
    <div class="box position-left-top">left top</div>
    <div class="box position-right-bottom">right bottom</div>
    <div class="box position-right-top">right top</div>

  

    <h2>Background Size 示例</h2>
    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: auto;
    ">background-size: auto（默认尺寸）</div>

  

    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: contain;
    ">background-size: contain（等比缩放，完整显示）</div>


    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: cover;
    ">background-size: cover（等比缩放，填满容器，可能裁剪）</div>

  

    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: 300px 300px;
    ">background-size: 300px 300px（指定宽高）</div>

  


    <h2>Background Attachment 示例</h2>
    <div class="box_attachment" style="background-attachment: scroll;">
        background-attachment: scroll（背景跟页面一起滚动）<br><br>
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
    </div>


    <div class="box_attachment" style="background-attachment: local;">
        background-attachment: local（背景跟内容一起滚动）<br><br>
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
        内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。
    </div>


    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: cover;
        background-attachment: fixed;
    ">
        background-attachment: fixed（背景固定，不随页面滚动）
    </div>

  

    <h2>Background Clip 示例</h2>
    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: cover;
        border: 20px dashed black;
        background-clip: border-box;

    ">
        background-clip: border-box（默认，背景从边框开始绘制）
    </div>

  

    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: cover;
        border: 20px dashed black;
        background-clip: padding-box;
    ">
        background-clip: padding-box（背景从内边距开始绘制，不会覆盖边框）

    </div>

  

    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: center center;
        background-size: cover;
        border: 20px dashed black;
        padding: 40px;
        background-clip: content-box;
    ">
        background-clip: content-box（背景只在内容区域绘制）
    </div>

  

    <h2>Background Origin 示例</h2>
    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: top left;
        background-size: 300px 300px;
        border: 20px dashed black;
        padding: 40px;
        background-origin: border-box;
    ">
        background-origin: border-box（定位基于边框区域，背景从边框外沿开始计算）
    </div>

  

    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: top left;
        background-size: 300px 300px;
        border: 20px dashed black;
        padding: 40px;
        background-origin: padding-box;
    ">
        background-origin: padding-box（定位基于内边距区域，背景从padding区域起点开始计算）
    </div>

  

    <div class="box" style="
        background-image: url('images/test.jpg');
        background-repeat: no-repeat;
        background-position: top left;
        background-size: 300px 300px;
        border: 20px dashed black;
        padding: 40px;
        background-origin: content-box;

    ">
        background-origin: content-box（定位基于内容区域，背景从content区域起点开始计算）
    </div>

</body>

</html>
```

`images/test.jpg`:
![](file-20250416171340809.png)