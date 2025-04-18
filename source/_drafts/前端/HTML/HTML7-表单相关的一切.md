# 一.本节代码
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>HTML表单示例</title>
</head>
<body>
    <form action="html文本.html" method="post">
        <label for="name">姓名：</label>
        <input type="text" id="name" name="username" required placeholder="请输入姓名">
        <br><br>
        <label for="pwd">密码：</label>
        <input type="password" id="pwd" name="password" required>
        <br><br>
        <label>性别：</label>
        <input type="radio" name="gender" value="male">男
        <input type="radio" name="gender" value="female">女
        <br><br>
        <label>爱好：</label>
        <input type="checkbox" name="hobby" value="reading">阅读
        <input type="checkbox" name="hobby" value="sports">运动
        <br><br>
        <label for="city">城市：</label>
        <select id="city" name="city">
          <option value="bj">北京</option>
          <option value="sh">上海</option>
          <option value="gz">广州</option>
        </select>
        <br><br>
        <label>个人简介：</label>
        <textarea name="about" rows="4" cols="30"></textarea>
        <br><br>
        <button type="submit">提交</button>
        <button type="reset">重置</button>
    </form>      
</body>
</html>
```
# 二.基本结构

```html
<form action="处理地址" method="提交方式">
  <!-- 表单元素 -->
</form>
```

| 属性       | 作用                                      |
| -------- | --------------------------------------- |
| `action` | 表单数据提交的地址（服务器接口或本地文件）                   |
| `method` | 数据提交方式：`GET`（默认，拼接URL） 或 `POST`（数据在请求体） |

**action:**

|属性值类型|示例|说明|
|---|---|---|
|本地文件路径|`action="submit.html"`|点击提交后跳转到这个本地页面，但无法真正处理表单数据。|
|后台接口地址（相对路径）|`action="submit.php"`|数据会被提交到服务器端PHP程序进行处理。|
|后台接口地址（绝对路径）|`action="https://abc.com/api/form"`|数据提交给远程服务器进行处理。|

**method:**

| 提交方式   | 特点                                             | 适用场景                |
| ------ | ---------------------------------------------- | ------------------- |
| `GET`  | 数据附加在 URL 后面（地址栏可见，如变成search.php?keyword=HTML） | 查询操作，调试、搜索框。        |
| `POST` | 数据放在请求体中，地址栏看不见                                | 提交重要信息（如账号密码、注册表单）。 |


#### 🚀 `GET` 示例：
# 三.常用表单标签

| 标签           | 作用     | 说明             |
| ------------ | ------ | -------------- |
| `<input>`    | 用户输入框  | 文本框、密码框、按钮等都用它 |
| `<textarea>` | 多行文本输入 | 用户可以输入多行文字     |
| `<select>`   | 下拉选择框  | 用于选择一项或多项      |
| `<option>`   | 下拉框选项  | 配合`<select>`使用 |
| `<button>`   | 按钮     | 提交/重置/普通按钮     |
| `<label>`    | 标签     | 绑定表单项，提升可用性    |


# 四.`<input>` 的类型

```html
<input type="text">         <!-- 单行文本输入 -->
<input type="password">     <!-- 密码输入框 -->
<input type="email">        <!-- 邮箱输入框，自动格式校验 -->
<input type="number">       <!-- 数值输入 -->
<input type="checkbox">     <!-- 多选框 -->
<input type="radio">        <!-- 单选按钮 -->
<input type="submit">       <!-- 提交按钮 -->
<input type="reset">        <!-- 重置按钮 -->
<input type="file">         <!-- 上传文件 -->
<input type="hidden">       <!-- 隐藏数据 -->
<input type="date">         <!-- 日期选择 -->
<input type="color">        <!-- 颜色选择器 -->
<input type="search">       <!-- 搜索框 -->
```


# 五.`<textarea>` 多行文本

```html
<textarea rows="4" cols="50">默认文本</textarea>
```
- `rows`：行数
- `cols`：列数

# 六.`<select>` 下拉列表

```html
<select name="城市">
  <option value="bj">北京</option>
  <option value="sh">上海</option>
  <option value="gz" selected>广州</option>
</select>
```
- `selected`：默认选中
- 可以通过 `multiple` 允许多选。

# 七.`<label>` 标签

绑定表单元素，提高可点击区域，增强可访问性。
```html
<label for="username">用户名：</label>
<input id="username" type="text">
```

- `for` 属性值要与目标控件的 `id` 对应。
- `for="username"`的作用：  绑定对应的输入框，**当用户点击“用户名”这两个字时，输入焦点会自动跳到下面的`<input>`框**，让表单更方便使用。
- `id="username"`：给输入框设置一个唯一的标识符，方便与 `<label>`、JavaScript、CSS关联。


# 八.按钮类型

|标签|作用|
|---|---|
|`<button type="submit">提交</button>`|提交表单|
|`<button type="reset">重置</button>`|重置表单|
|`<button type="button">普通按钮</button>`|无提交功能，配合JS使用|

# 九.表单属性

| 属性            | 说明                         |
| ------------- | -------------------------- |
| `name`        | 表单项名称，提交时数据的键              |
| `value`       | 表单项的值                      |
| `placeholder` | 提示占位符                      |
| `required`    | 必填                         |
| `readonly`    | 只读                         |
| `disabled`    | 禁用                         |
| `checked`     | 默认勾选（用于`checkbox`和`radio`） |
| `maxlength`   | 最大字符数限制                    |
| `pattern`     | 正则表达式验证                    |

#  十.表单验证
1. HTML内置验证属性：
    - `required`：是否必填；
    - `pattern`：正则表达式约束；
    - `min` / `max`：数值大小范围；
    - `maxlength`：最大输入字符；
    - `type="email" / "url"`：自动格式验证。
2. 配合JavaScript自定义验证