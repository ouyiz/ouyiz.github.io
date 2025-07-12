## 一、原始数据类型（Primitive Types）
共 7 种（从 ES6 起）：

|类型|示例|说明|
|---|---|---|
|`Number`|`123`、`3.14`、`NaN`|所有数字（整数/小数）|
|`String`|`"hello"`、`'hi'`|文本内容|
|`Boolean`|`true`、`false`|布尔值（逻辑判断）|
|`Undefined`|`let x;`|声明未赋值的变量|
|`Null`|`let x = null;`|空值，表示“无”或“清空”|
|`Symbol`|`Symbol("id")`|独一无二的标识符（ES6）|
|`BigInt`|`1234567890123456789012345n`|表示非常大的整数（ES2020）|

## 二、引用数据类型（Reference Types）
引用类型的值是**对象的引用地址**，包括：

| 类型       | 示例                       |
| -------- | ------------------------ |
| Object   | `{name: "Tom", age: 18}` |
| Array    | `[1, 2, 3]`              |
| Function | `function() {}`          |
| Date     | `new Date()`             |
| RegExp   | `/abc/`                  |

##  三、类型判断方法

```js
typeof 123        // "number"
typeof "abc"      // "string"
typeof null       // "object" （设计上的历史 bug）
typeof undefined  // "undefined"
typeof []         // "object"
typeof {}         // "object"
typeof function(){} // "function"

Array.isArray([]) // true
```

##  四、类型转换
**1.转为字符串（String）**
```js
String(123);     // "123"
123 + "";        // "123"
```

**2.转为数字（Number）**
```js
Number("123");   // 123
parseInt("123.45");  // 123
+"456";          // 456
```

**3.转为布尔值（Boolean）**
```js
Boolean("");     // false
Boolean("abc");  // true
Boolean(0);      // false
```


## 五、动态类型
JavaScript 拥有动态类型，变量在声明时不需要指定类型，运行过程中类型可以改变。
```javascript
let x = 10;         // 现在 x 是一个 Number
x = "hello";        // 现在 x 变成了 String
x = true;           // 现在 x 又变成了 Boolean
```

## 六、对象
在 JavaScript 中，对象是一种键值对的集合，用于表示一个具有属性和行为的实体。
```javascript
let person = {
  name: "Alice",     // 属性（key: value）
  age: 25,
  sayHi: function() {  // 方法（函数）
    console.log("Hi!");
  }
};
```

**对象的特点：**
- 使用 `{}` 创建；
- 每个属性都是 `键（字符串）: 值` 的形式；
- 值可以是任意类型（字符串、数字、数组、函数、其他对象）；
- 可以动态添加、删除属性。

**访问对象属性**
```javascript
console.log(person.name);     // 点语法：Alice
console.log(person["age"]);   // 方括号语法：25
```

**操作对象属性**
```javascript
// 修改属性
person.age = 30;

// 新增属性
person.gender = "female";

// 删除属性
delete person.name;
```

**对象方法（函数作为属性）**
```javascript
let dog = {
  name: "Buddy",
  bark: function() {
    console.log("Woof!");
  }
};

dog.bark();  // 调用方法
```

**遍历对象属性**
```javascript
for (let key in person) {
  console.log(key + ": " + person[key]);
}
```
