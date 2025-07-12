
## 一、定义一个类
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  sayHi() {
    console.log(`你好，我是 ${this.name}`);
  }
}

const p = new Person("小明", 18);
p.sayHi(); // 你好，我是 小明
```
- `class`：定义类的关键字。
- `constructor`：构造方法，创建对象时自动调用。
- `this`：指向当前对象。
- `sayHi()`：类的方法，不需要 `function` 关键字。

## 二、类的继承（extends）
一个类可以继承另一个类，子类可以使用 `super` 调用父类构造器或方法：
```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} 发出声音`);
  }
}

class Dog extends Animal {
  constructor(name, color) {
    super(name); // 调用父类构造函数
    this.color = color;
  }

  speak() {
    console.log(`${this.name} 汪汪叫`);
  }
}

const dog = new Dog("旺财", "黑白色");
dog.speak(); // 旺财 汪汪叫
```


## 三、成员可见性
**1.公有成员（默认）**
```js
class Car {
  brand = "Toyota";

  drive() {
    console.log("开车！");
  }
}
```

**2.私有属性（用 `#` 声明，ES2022 标准）**
只能在类的内部访问，类外部不能直接访问或修改。
```js
class Secret {
  #code = 1234;

  revealCode() {
    console.log(this.#code);
  }
}

const s = new Secret();
s.revealCode(); // 1234
// console.log(s.#code); // ❌ 报错：私有属性不可访问
```


## 四、静态属性和方法（static）
静态成员属于“类”，而不是“对象实例”：
```js
class MathUtil {
  static PI = 3.14159;

  static square(x) {
    return x * x;
  }
}

console.log(MathUtil.PI);        // 3.14159
console.log(MathUtil.square(5)); // 25
```
- 静态方法不能通过对象调用，只能通过类本身访问。


## 五、类表达式（Class Expressions）
你也可以用表达式来创建类，类似函数表达式：
```js
const MyClass = class {
  sayHi() {
    console.log("Hi");
  }
};

new MyClass().sayHi(); // Hi
```

## 六、super 关键字
- 用于调用父类的构造器或方法。
- 构造器中必须先调用 `super()` 才能使用 `this`。
	- 当你使用 `extends` 继承父类时，子类还没有“完全初始化”，所以 JavaScript 要你先调用 `super()` 来初始化父类的部分，这样系统才能安全地初始化 `this`。
```js
class A {
  hello() {
    console.log("A says hello");
  }
}

class B extends A {
  hello() {
    super.hello(); // 调用父类方法
    console.log("B says hello");
  }
}

class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, color) {
    this.color = color;     // ❌ 错误：没有先调用 super()
    super(name);            // 必须放在前面！
  }
}

```

