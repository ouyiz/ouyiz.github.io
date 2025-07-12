##  一、核心属性：`transform`
 `transform` 属性用于应用 2D 或 3D 变换，如旋转、缩放、平移、倾斜等。
```css
transform: <transform-function> [, <transform-function>]*;
```
可组合多个变换函数，按从右到左执行。

## 二、2D 变换函数

**1.`translate(x, y)`**
- 沿X轴和Y轴平移
```css
transform: translate(50px, 100px);
```

**2.`translateX(x)`**
- 沿X轴平移
```css
transform: translateX(50px);
```

**3.`translateY(y)`**
- 沿Y轴平移
```css
transform: translateY(50px);
```

 **4.`scale(x, y)`**
- 按比例缩放元素（默认中心点缩放）
```css
transform: scale(1.5, 0.5);
```

**5.`scaleX(x)`**
- 沿X轴缩放
```css
transform: scaleX(1.2);
```

**6.`scaleY(y)`**
- 沿Y轴缩放
```css
transform: scaleY(0.8);
```

**7.`rotate(angle)`**
- 以中心为轴顺时针旋转
```css
transform: rotate(45deg);
```

**8.`skew(x-angle, y-angle)`**
- 倾斜变换
```css
transform: skew(20deg, 10deg);
```

**9.`skewX(angle)` / `skewY(angle)`**
- 沿X或Y轴倾斜
```css
transform: skewX(15deg);
transform: skewY(10deg);
```

**10.`matrix(a, b, c, d, e, f)`**
- 用 2D 矩阵表示平移、缩放、旋转、倾斜的组合（很难）
```css
transform: matrix(1, 0, 0, 1, 100, 100);
```

## 三、3D 变换函数
**1.`translateZ(z)`**
- 沿Z轴移动
```css
transform: translateZ(100px);
```

**2.`translate3d(x, y, z)`**
- 在三维空间中平移
```css
transform: translate3d(50px, 50px, 50px);
```

**3.`scaleZ(z)`**
- 沿Z轴缩放
```css
transform: scaleZ(1.5);
```

**4.`scale3d(x, y, z)`**
- 三轴缩放
```css
transform: scale3d(1.2, 1.2, 1.2);
```

**5.`rotateX(angle)`**
- 绕X轴旋转
```css
transform: rotateX(45deg);
```

**6.`rotateY(angle)`**
- 绕Y轴旋转
```css
transform: rotateY(60deg);
```

**7.`rotateZ(angle)`**
- 绕Z轴旋转（= rotate）
```css
transform: rotateZ(90deg);
```

**8.`rotate3d(x, y, z, angle)`**
- 绕自定义轴旋转
```css
transform: rotate3d(1, 1, 0, 45deg);
```

**9.`matrix3d(n1, n2, ..., n16)`**
- 使用16参数构成的3D矩阵（难）
```css
transform: matrix3d(…); /* 很少手动使用 */
```

## 四、变换控制属性

**1.`transform-origin`**
- 设置变换中心点（影响旋转、缩放等）
```css
transform-origin: center center;
transform-origin: 0% 0%; /* 左上角 */
transform-origin: 50% 100%; /* 中下 */
```
3D中可加Z轴：
```css
transform-origin: 50% 50% 100px;
```

**2.`transform-style`**
- 控制子元素是否保留3D变换上下文
```css
transform-style: flat; /* 默认，忽略子元素3D效果 */
transform-style: preserve-3d; /* 子元素保留3D */
```

**3.`perspective`**
- 设置透视距离（通常用于父容器）
```css
perspective: 800px;
```
距离越小，透视感越强。

**4.`perspective-origin`**
- 设置观察者视角
```css
perspective-origin: center center;
```

**5.`backface-visibility`**
- 设置背面是否可见（3D翻转时常用）
```css
backface-visibility: visible; /* 默认 */
backface-visibility: hidden; /* 背面隐藏 */
```

## 五、本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CSS Transform 效果演示</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      padding: 20px;
    }

    h2 {
      margin-top: 60px;
      border-bottom: 2px solid #ccc;
      padding-bottom: 10px;
    }

    .demo {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      margin-top: 20px;
    }

    .box {
      width: 100px;
      height: 100px;
      background: #4da3ff;
      color: white;
      font-weight: bold;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 8px;
      transition: all 0.6s ease;
      cursor: pointer;
    }

    .demo .box:hover {
      background: #003d99;
    }

    /* 2D Transforms */
    .translate:hover { transform: translate(50px, 20px); }
    .scale:hover { transform: scale(1.5); }
    .rotate:hover { transform: rotate(45deg); }
    .skew:hover { transform: skew(20deg, 10deg); }
    .matrix:hover { transform: matrix(1, 0.2, 0.4, 1, 20, 10); }

    /* 3D Transforms */
    .rotateX:hover { transform: rotateX(180deg); }
    .rotateY:hover { transform: rotateY(180deg); }
    .rotateZ:hover { transform: rotateZ(90deg); }
    .translateZ:hover { transform: translateZ(50px); }
    .scale3d:hover { transform: scale3d(1.2, 1.2, 1.2); }
    .rotate3d:hover { transform: rotate3d(1, 1, 0, 60deg); }

    .perspective-container {
      perspective: 800px;
    }

    .box3d {
      transform-style: preserve-3d;
    }

    .backface {
      backface-visibility: hidden;
    }

    .flip-card {
      width: 100px;
      height: 100px;
      position: relative;
      transform-style: preserve-3d;
      transition: transform 0.8s;
    }

    /* 正面和背面 */
    .flip-front, .flip-back {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        color: white;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.2rem;
        border-radius: 12px;
    }

    /* 背面默认旋转180度 */
    .flip-back {
        transform: rotateY(180deg);
    }

    /* 1. 水平翻转悬停 */
    .flip-hover-y:hover {
        transform: rotateY(180deg);
    }

    /* 2. 垂直翻转悬停 */
    .flip-hover-x .flip-back {
        transform: rotateX(180deg);
    }
    .flip-hover-x:hover {
        transform: rotateX(180deg);
    }

    /* 3. 双面旋转悬停 */
    .flip-hover-xy .flip-back {
        transform: rotateY(180deg) rotateX(180deg);
    }
    .flip-hover-xy:hover {
        transform: rotateY(180deg) rotateX(180deg);
    }

    /* 4. 点击翻转触发 */
    .flip-click.flip-active {
        transform: rotateY(180deg);
    }
  </style>
</head>
<body>

  <h1>CSS Transform 效果演示</h1>

  <h2>2D Transform</h2>
  <div class="demo">
    <div class="box translate">translate</div>
    <div class="box scale">scale</div>
    <div class="box rotate">rotate</div>
    <div class="box skew">skew</div>
    <div class="box matrix">matrix</div>
  </div>

  <h2>3D Transform</h2>
  <div class="demo perspective-container">
    <div class="box rotateX">rotateX</div>
    <div class="box rotateY">rotateY</div>
    <div class="box rotateZ">rotateZ</div>
    <div class="box translateZ">translateZ</div>
    <div class="box scale3d">scale3d</div>
    <div class="box rotate3d">rotate3d</div>
  </div>

  <h2>3D 多样翻转卡片示例</h2>
    <div class="demo perspective-container" style="gap:40px;">

    <!-- 1. 水平翻转，悬停触发 -->
    <div class="flip-card flip-hover-y" style="width:120px; height:160px; cursor:pointer;">
        <div class="flip-front" style="background:#3498db;">Front Y</div>
        <div class="flip-back" style="background:#2980b9;">Back Y</div>
    </div>

    <!-- 2. 垂直翻转，悬停触发 -->
    <div class="flip-card flip-hover-x" style="width:120px; height:160px; cursor:pointer;">
        <div class="flip-front" style="background:#e67e22;">Front X</div>
        <div class="flip-back" style="background:#d35400;">Back X</div>
    </div>

    <!-- 3. 双面旋转，悬停触发 -->
    <div class="flip-card flip-hover-xy" style="width:120px; height:160px; cursor:pointer;">
        <div class="flip-front" style="background:#8e44ad;">Front XY</div>
        <div class="flip-back" style="background:#71368a;">Back XY</div>
    </div>

    <!-- 4. 点击触发翻转 -->
    <div class="flip-card flip-click" style="width:120px; height:160px; cursor:pointer;">
        <div class="flip-front" style="background:#27ae60;">Click Front</div>
        <div class="flip-back" style="background:#1e8449;">Click Back</div>
    </div>

    </div>

</body>
</html>

<script>
  document.querySelectorAll('.flip-click').forEach(card => {
    card.addEventListener('click', () => {
      card.classList.toggle('flip-active');
    });
  });
</script>
```



