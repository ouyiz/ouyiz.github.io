## 一.图片水平居中
`<img>`是`inline`元素
- 它不会独占一行；
- 它的宽度和高度由图片本身决定；

**方法一：**
- 想进行居中要使用`display: block;`
- 再使用`margin: 0 auto;`就会水平居中
```css
.profile-pic {
    width: 120px;          
    height: 120px;         
    object-fit: cover; 
    border-radius: 50%;
    display: block;  
    margin: 0 auto;  /*元素作为块级元素显示*/
    margin-top: 60px;     /*浏览器自动计算，一般用于水平居中*/ 
}
```

```html
<img src="https://s21.ax1x.com/2025/05/30/pV9ZMGR.jpg" class="profile-pic">
```

**方法二：**
`text-align` 是用来设置文本或行内元素（如 `<span>`、`<a>`、`<img>`）在其父容器中的水平对齐方式的 CSS 属性。
把 `<img>` 放进一个 `text-align: center` 的容器里就可以水平居中
```html
<div style="text-align: center;">
  <img src="xxx.jpg">
  <p>Hello world</p>
</div>
```

## 二.子元素水平垂直居中
下述CSS 将 `.contact-info` 区域变成了一个横向排列、水平居中、垂直对齐的弹性盒子容器
- `display: flex`
	- 容器会自动把所有子元素当作弹性项目进行排列，默认是横向一行排列。
- `justify-content: center;`
	- 表示所有子元素在主轴方向上居中对齐
	- 在 `flex` 默认方向下，主轴是水平方向
- `align-items: center;`
	- 交叉轴是与主轴垂直的方向，默认是垂直方向。
	- `align-items: center` 表示所有子元素在垂直方向上**垂直居中**。
```css
.contact-info {
    display: flex;
    gap: 15px;
    color: #eee;
    justify-content: center;   
    align-items: center;       
}
```