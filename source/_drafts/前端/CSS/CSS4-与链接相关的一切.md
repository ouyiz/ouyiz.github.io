
# 1.链接伪类

|伪类|作用|说明|
|---|---|---|
|`a:link`|未访问链接|默认状态，尚未点击的链接|
|`a:visited`|已访问链接|用户点击访问过的链接|
|`a:hover`|鼠标悬停|鼠标悬停在链接上的状态|
|`a:active`|激活链接|鼠标按下（点击瞬间）时的状态|
|`a:focus`|获取焦点|链接通过键盘或鼠标聚焦时|

⚠️ `:focus`经常与无障碍设计、键盘操作相关，别忽略。

---

### ② 链接文字样式相关属性

|属性|作用|常用值|
|---|---|---|
|`color`|链接文字颜色|支持十六进制、RGB、英文名等|
|`text-decoration`|装饰效果|`none`（无） / `underline`（下划线，默认值）/ `overline` / `line-through`|
|`font-weight`|链接文字粗细|`normal` / `bold` / `bolder` / `lighter` 或数值|

---

### ③ 链接交互效果相关

|属性|作用|示例|
|---|---|---|
|`cursor`|鼠标指针样式|`pointer`（常用，表示可点击）|
|`transition`|平滑动画过渡|比如 `transition: all 0.3s ease;` 实现悬停平滑变色|
|`outline`|焦点外边框|`outline: none;` 可取消默认聚焦边框，建议与`:focus`结合|

---

### ④ 链接布局相关

- `display`  
    默认a标签是 `inline`，也可以设置为：
    

```
display: block;
display: inline-block;
```

配合 `padding`，可以把链接做成按钮样式。

---

### ⑤ 链接安全相关（HTML属性）

虽然属于HTML，但和CSS一起用得很多：

|属性|说明|
|---|---|
|`target="_blank"`|链接新窗口打开|
|`rel="noopener noreferrer"`|防止 `target="_blank"` 被钓鱼劫持，安全性最佳实践|

配合CSS：

```css
a[target="_blank"]::after {
    content: " 🔗"; /* 给外链加个小图标 */
}
```

---

### 💡 小总结：

> **链接相关的CSS知识** = 状态伪类 + 样式属性 + 交互效果 + 布局属性 + 配合安全性。
> 
> 做到：  
> 美观性（颜色、装饰）＋  
> 交互性（hover、focus）＋  
> 可访问性（focus样式）＋  
> 安全性（rel属性）。

---

如果需要，我可以： ✅ 帮你整理一个完整**“a链接美化实战版”案例代码**；  
✅ 或者帮你画一张知识结构图，方便记忆！

要我帮你整理一下吗？🎯