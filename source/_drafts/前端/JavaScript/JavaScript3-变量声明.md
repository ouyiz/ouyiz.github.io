## 一、变量的基本概念
变量（Variable） 是用来存储数据的容器。你可以给它取个名字，然后赋值。
```javascript
let name = "小明";
let age = 18;
```

## 二、声明变量的三种方式
**1.`var`（ES5 及以前的方式）**
```javascript
var x = 5;
```
- 可以重复声明
- 作用域是函数级，不是块级

**2.`let`（ES6 新增）**
```javascript
let x = 10;
```
- 块级作用域（block scope）
-  不能重复声明

**3.`const`（ES6 新增）**
```javascript
const PI = 3.14;
```
- 块级作用域
- 不能重复声明
- 必须初始化

## 三、变量命名规则
变量名必须遵循以下规则：
- 只能包含 **字母、数字、下划线`_`、$ 符号**
- 不能以数字开头
- 区分大小写
- 避免使用 JavaScript 的关键字（如 `if`, `for`, `while`, `let`）

## 四、作用域
**作用域（Scope）**：就是变量可以被访问的“范围”或“区域”。
#### 1.函数级作用域
只有函数是一个作用域单元，`if`、`for`、`while` 这些语句 **不是作用域**。
**示例：**
```javascript
function test() {
  var a = 10; // 变量 a 属于 test 函数这个作用域

  if (true) {
    var a = 20; // 还是同一个变量 a，被重新赋值
  }

  console.log(a); // 输出 20
}
test();
```

#### 2.块级作用域（Block Scope）
所有由 `{}` 包围的代码块（比如 `if`、`for`、`while`、`{} 本身`）都是一个作用域。
**示例：**
```javascript
function test() {
  let x = 1;

  if (true) {
    let x = 2; // 这是另一个变量，仅在这个 if 代码块中有效
    console.log("块内：", x); // 2
  }

  console.log("块外：", x); // 1
}
test();
```
