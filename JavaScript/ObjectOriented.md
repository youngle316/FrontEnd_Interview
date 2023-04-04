# JS JS 基础-面向对象

### 问：说一下你对面向对象的理解

答：面向对象中有两个很重要的概念就是对象和类，类是对一类事物的总称（比如说狗），对象就是某一个具体的狗。在 `JS` 中创建类的方式在 `ES6` 之前是使用 `Function`，`function Dog() {}`，在 `ES6` 引入了 `class` 这个关键字来表示类。创建对象其中一种方式就是通过 `new` 关键字，通过 `new` 构造函数创建一个一个的对象。要知道某个对象是属于哪个类，这时就涉及到原型和原型链，当`对象.__proto__ === 类.prototype`时，就可以说这个对象属于该类。

### 问：创建对象有几种方法

答：有三种

1. 对象字面量
   ```javascript
   let obj = { name: "obj" };
   let obj1 = new Object({ name: "obj" });
   ```
2. 使用显示的构造函数创建对象
   ```javascript
   let Fun = function () {
     this.name = "obj";
   };
   let obj = new Fun();
   ```
3. `Object.create()`
   ```javascript
   let P = { name: "obj" };
   let obj = Object.create(p);
   ```

### 问：说一下原型链

答：

![Prototype](https://obsidian-picgo-le.oss-cn-hangzhou.aliyuncs.com/img/SCR-20230403-o4d.png)

原型链是 `JavaScript` 内部实现继承的机制。

原型链上包含了三部分，分别为构造函数，实例和原型对象。构造函数通过 `new` 运算符生成了实例对象，构造函数在创建时 `JS` 引擎会自动给加上 `prototype`，指向其原型对象。原型对象可以通过 `constructor` 获取构造器。实例通过 `__proto__` 指向其构造函数的原型对象。

当我们访问一个对象的属性或者方法时，如果该对象本身没有这个属性或方法时，就会沿着原型链一直往上找，直到找到或者到达原型链的顶端 `null`

### 问：instanceof 的原理

答：`instanceof` 用来判断构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。其原理就是通过原型链，判断实例对象的 `__proto__` 是否等于构造函数的 `prototype` 属性，如果不等于再继续通过原型链找原型对象的原型对象，直到找到顶端 `null`

### 问：`new` 运算符都做了什么

答：new 运算符

1. 新的对象被创建，该对象继承自构造函数的 `prototype` 属性
2. 执行构造函数。在执行期间，将 `this` 关键字绑定到实例对象上，以便构造函数可以修改其属性。
3. 如果构造函数具有返回值且返回值是一个对象，则返回该对象。否则，返回创建的实例对象。

### 问：自己实现一个 `new` 运算符

答：

```javascript
function myNew(constructor, ...arg) {
  // 新的对象被创建，该对象继承自构造函数的 `prototype` 属性
  const instance = Object.create(constructor.prototype);
  // 执行构造函数。在执行期间，将 `this` 关键字绑定到实例对象上，以便构造函数可以修改其属性。
  const result = constructor.apply(instance, arg);
  // 如果构造函数具有返回值且返回值是一个对象，则返回该对象。否则，返回创建的实例对象。
  if (typeof result === "object" && result !== null) {
    return result;
  }
  return instance;
}
```

### 问：call apply bind 的区别

答：这三者都可以改变 `this` 的指向，区别在与 `call` 和 `apply` 都是立即执行函数，只是传参方式不同，`call` 方法接受一组参数列表，`apply` 接受一个参数数组。

```javascript
function greet(name) {
  console.log(`Hello, ${name}! My name is ${this.name}.`);
}

const person1 = { name: "Alice" };
const person2 = { name: "Bob" };

greet.call(person1, "David"); // 输出：Hello, David! My name is Alice.
greet.apply(person2, ["Emily"]); // 输出：Hello, Emily! My name is Bob.
```

`bind` 方法返回一个函数，该函数与原函数具有相同的函数体，但是内部的 `this` 值已经被绑定为指定的值

```javascript
const greetPerson1 = greet.bind(person1);
const greetPerson2 = greet.bind(person2);

greetPerson1("Frank"); // 输出：Hello, Frank! My name is Alice.
greetPerson2("Grace"); // 输出：Hello, Grace! My name is Bob.
```

### 问：如何实现继承

答：`ES6` 直接使用 `class` 和 `extends` 即可。`ES5` 使用组合继承

```javascript
function Parent() {
  this.name = "parent";
  this.play = [1, 2, 3];
}

function Child() {
  this.name = "child";
  this.type = "child";
}

// Object.create 的作用相当于 Child.prototype.__proto__ = Parent.prototype
Child.prototype = Object.create(Parent.prototype);

Child.prototype.constructor = Child;
```
