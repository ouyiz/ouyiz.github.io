# 一.本节代码
```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>学生成绩表</title>
</head>
<body>
    <table>
        <tr>
          <th>姓名</th>
          <th>年龄</th>
        </tr>
        <tr>
          <td>张三</td>
          <td>20</td>
        </tr>
      </table>
      
      <hr>

      <table border="1">
        <caption>学生信息表</caption>
        <thead>
          <tr><th>姓名</th><th>年龄</th></tr>
        </thead>
        <tbody>
          <tr><td>张三</td><td>20</td></tr>
          <tr><td>李四</td><td>21</td></tr>
        </tbody>
        <tfoot>
          <tr><td>共2人</td><td></td></tr>
        </tfoot>
      </table>
      
      <hr>
      
    <table border="1" cellpadding="5" cellspacing="0" style="border-collapse: collapse; width: 80%;">
    <caption>商品库存表</caption>
    <thead>
        <tr>
            <th>商品名</th>
            <th>单价</th>
            <th>库存</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>苹果</td>
            <td>¥3.5</td>
            <td>120</td>
        </tr>
        <tr>
            <td>香蕉</td>
            <td>¥2.0</td>
            <td>80</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="2">总库存</td>
            <td>200</td>
        </tr>
    </tfoot>
    </table>
    
</body>
</html>

```

# 二.表格的基本结构

HTML使用 `<table>` 标签创建表格，常见结构如下：

|标签|作用|
|---|---|
|`<table>`|创建表格|
|`<tr>`|表格的行（table row）|
|`<td>`|表格的单元格（table data）|
|`<th>`|表头单元格（table header）|

 例子：

```html
<table>
  <tr>
    <th>姓名</th>
    <th>年龄</th>
  </tr>
  <tr>
    <td>张三</td>
    <td>20</td>
  </tr>
</table>
```


# 三.表格结构补充标签

|标签|作用|
|---|---|
|`<thead>`|表头区，包含表头行 `<tr>`|
|`<tbody>`|表格主体，包含数据行 `<tr>`|
|`<tfoot>`|表格底部，通常放总结行|
|`<caption>`|表格标题（一般在表格上方）|
结构示范：

```html
<table>
  <caption>学生信息表</caption>
  <thead>
    <tr><th>姓名</th><th>年龄</th></tr>
  </thead>
  <tbody>
    <tr><td>张三</td><td>20</td></tr>
    <tr><td>李四</td><td>21</td></tr>
  </tbody>
  <tfoot>
    <tr><td>共2人</td><td></td></tr>
  </tfoot>
</table>
```



# 四.单元格合并

|属性|作用|示例|
|---|---|---|
|`colspan`|合并列（横向合并）|`<td colspan="2">`|
|`rowspan`|合并行（纵向合并）|`<td rowspan="2">`|

# 五.表格的属性

| 属性            | 功能说明          | 示例                          |
| ------------- | ------------- | --------------------------- |
| `border`      | 设置表格边框        | `<table border="1">`        |
| `cellpadding` | 单元格内部内容与边框的距离 | `<table cellpadding="10">`  |
| `cellspacing` | 单元格之间的间距      | `<table cellspacing="5">`   |
| `width`       | 表格宽度          | `<table width="500">` 或 `%` |
| `height`      | 表格高度          | `<table height="300">`      |
| `align`       | 表格整体的水平对齐方式   | `left / center / right`     |
| `bgcolor`     | 表格背景色         | `<table bgcolor="#eee">`    |

⚠️现代开发推荐使用**CSS控制样式**，而不是直接用这些属性！

# 六.表格注意事项

1. `<th>`默认文字加粗并居中，强调表头作用；
2. `<td>`默认文字居左，普通数据单元格；


# 七.练习

## 练习1：完成一个学生成绩表格（使用 `<table>`、`<tr>`、`<td>` 和 `<th>`）
要求：
- 表头有：姓名、数学、英语、总分。
- 至少填写2名学生成绩。    

## 练习2：为表格添加表头、表体、表脚结构（`<thead>`、`<tbody>`、`<tfoot>`）
要求：
- `<thead>`中显示列名。
- `<tfoot>`显示“共计2人”。


## 练习3：设置表格属性
要求：
- `border="1"`，`cellpadding="5"`，`cellspacing="10"`；
- 表格宽度设置为 `600`；
- 表格整体居中显示。

## 练习4：合并单元格
要求：
- 使用 `colspan` 横向合并标题；
- 使用 `rowspan` 合并一个单元格，模拟同一个人参加不同科目的情况。


## 答案

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>表格练习标准答案</title>
</head>
<body>

<!-- 表格练习1 + 练习2 -->
<table border="1" cellpadding="5" cellspacing="10" width="600" align="center">
  <caption>学生成绩表</caption>

  <thead>
    <tr>
      <th>姓名</th>
      <th>数学</th>
      <th>英语</th>
      <th>总分</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>张三</td>
      <td>90</td>
      <td>85</td>
      <td>175</td>
    </tr>
    <tr>
      <td>李四</td>
      <td>88</td>
      <td>92</td>
      <td>180</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <td colspan="4">共计2人</td>
    </tr>
  </tfoot>
</table>

<br>

<!-- 表格练习4：合并单元格示范 -->
<table border="1" cellpadding="5" cellspacing="5" align="center">
  <caption>考试成绩单</caption>

  <tr>
    <th colspan="3">成绩汇总</th>
  </tr>
  <tr>
    <th>姓名</th>
    <th>科目</th>
    <th>分数</th>
  </tr>
  <tr>
    <td rowspan="2">王五</td>
    <td>数学</td>
    <td>95</td>
  </tr>
  <tr>
    <td>英语</td>
    <td>89</td>
  </tr>
</table>

</body>
</html>
```
