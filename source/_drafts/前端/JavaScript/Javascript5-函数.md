## 一、函数的定义方式
**1.函数声明（命名函数）**
```javascript
function sayHello() {
  console.log("Hello!");
}
```

**2 函数表达式（匿名函数赋值给变量）**
```javascript
const sayHi = function() {
  console.log("Hi!");
};
```

**3.箭头函数（ES6）**
```javascript
const add = (a, b) => a + b;
```
- 如果只有一个参数，可以省略括号；
- 如果只有一条语句，且是返回值，可以省略 `return` 和大括号。



## 二、函数的调用
```javascript
sayHello();   // 调用函数
let sum = add(3, 4);  // 传参并获取返回值
```


## 三、函数参数
- JavaScript 中的函数参数**不需要指定类型**；
- 参数个数可以**不固定**；
- 如果参数未传，默认为 `undefined`。
```javascript
function greet(name) {
  console.log("Hello, " + name);
}
greet("Alice"); // Hello, Alice
greet();        // Hello, undefined
```

## 四、返回值
函数可以使用 `return` 返回一个值：
```javascript
function multiply(a, b) {
  return a * b;
}
let result = multiply(2, 5);  // 10
```

## 五、函数作用域（Scope）
- 函数内部声明的变量是**局部变量**，外部无法访问；
- 外部的变量在函数内部可以访问（闭包）；
- JavaScript 支持**函数嵌套定义**。
```javascript
let x = 10;

function test() {
  let y = 20;
  console.log(x); // 可以访问外部变量 x
  console.log(y); // 只能在函数内部访问
}
```

## 六、arguments 对象
所有函数都可以使用内置的 `arguments` 对象访问**传入的所有参数**：
```javascript
function sumAll() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}
sumAll(1, 2, 3, 4); // 返回 10
```
注意：箭头函数 **没有 arguments 对象**。

## 七、默认参数（ES6）
```javascript
function greet(name = "Guest") {
  console.log("Hello, " + name);
}
greet();            // Hello, Guest
greet("Alice");     // Hello, Alice
```

## 八、函数作为值（回调函数）
函数可以作为参数传递给另一个函数：
```javascript
function process(callback) {
  callback();
}

process(function() {
  console.log("回调函数被调用");
});
```


## 九、函数闭包（Closure）
闭包 = 函数 + 它能访问的外部变量
当一个函数内部定义的函数，引用了外部函数的变量时，就会形成一个“闭包”。

**例子：**
```javascript
function outer() {
  let count = 0; // 外部函数的局部变量

  function inner() {
    count++;
    console.log(count);
  }

  return inner; // 返回内部函数
}

let fn = outer();  // 执行外部函数，返回内部函数
fn(); // 输出 1
fn(); // 输出 2
```
- `outer()` 执行时，`count` 是它的局部变量；
- `inner()` 是内部函数，**使用了 `count`**；
- 即使 `outer()` 已经结束，`count` 变量并**不会被销毁**，因为 `inner()` **还在使用它**；
- 这就是闭包：`inner()` 函数和它“记住”的外部变量 `count`。


**作用：**
1.记住状态
可以创建有“记忆”的函数，比如计数器：
```javascript
function createCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

let counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

2.封装私有变量
外部不能直接访问闭包中的变量，只能通过返回的函数操作变量：
```javascript
function Secret() {
  let secret = "123456";
  return {
    get: function() { return secret; },
    set: function(val) { secret = val; }
  };
}

let s = Secret();
console.log(s.get()); // 123456
s.set("abc");
console.log(s.get()); // abc
```
