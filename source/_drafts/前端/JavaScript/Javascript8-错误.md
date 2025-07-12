在 JavaScript 中，错误（Error）是程序运行时出现的异常。理解如何识别、处理和抛出错误是开发中很重要的一部分。

## 一. 常见错误类型
JavaScript 提供了一些内置的错误对象，用于描述不同类型的错误：

|错误类型|说明|示例|
|---|---|---|
|`Error`|基础错误类型，其他错误类型都继承它|`new Error("出错了")`|
|`SyntaxError`|语法错误|`eval("var a = )")`|
|`ReferenceError`|引用了未声明的变量|`console.log(x)`（x未定义）|
|`TypeError`|类型错误，比如方法用错|`null.f()`|
|`RangeError`|值超出了允许范围|`new Array(-1)`|
|`URIError`|URI 编码错误|`decodeURIComponent('%')`|
|`EvalError`|`eval()` 使用错误（现在很少出现）|`throw new EvalError()`|

## 二.Error 对象基本结构
创建一个错误对象：
```js
let error = new Error("出错信息");
console.log(error.name);    // Error
console.log(error.message); // 出错信息
```
你也可以使用子类：
```js
let err = new TypeError("类型不对");
console.log(err.name);    // TypeError
console.log(err.message); // 类型不对
```

## 三.try...catch...finally 用法（错误捕获）
用 `try...catch` 来“**捕获并处理**”运行时错误，防止程序崩溃：
```js
try {
  let x = y + 1; // y 未定义
} catch (err) {
  console.log("出错了：", err.message);
}
```

finally（无论有没有错误都会执行）
```js
try {
  console.log("开始");
  throw new Error("问题出现了！");
} catch (e) {
  console.log("捕获错误：", e.message);
} finally {
  console.log("无论如何都会执行");
}
```

## 四.throw 抛出错误
你可以使用 `throw` 主动抛出错误，让程序中断或转入 `catch`：
```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("除数不能为 0");
  }
  return a / b;
}

try {
  divide(10, 0);
} catch (err) {
  console.log("错误信息：", err.message);
}
```
你还可以抛出任何类型的值（虽然最好使用 `Error` 对象）：
```js
throw "简单字符串错误";
throw 123;
```

## 五.自定义错误类（进阶）
你可以自己定义一个错误类型：
```js
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = "MyError";
  }
}

throw new MyError("这是我自己定义的错误");
```


## 六.开发建议
- 永远使用 `try...catch` 来**安全地执行可能失败的代码**
- 使用 `throw new Error(...)` 来明确抛出错误
- 写代码时尽量明确区分语法问题、类型问题等
