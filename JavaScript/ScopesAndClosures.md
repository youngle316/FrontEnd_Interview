# JS 基础-作用域与闭包

### 问：什么是作用域，什么是自由变量

答：作用域分为全局作用域，函数作用域和块级作用域。作用域就是变量或者函数可以被访问的范围。自由变量指在函数内部使用但未被定义的变量，值由外层函数或者全局作用域提供。

### 问：什么是闭包

答：

1. 闭包是可以获取其他函数内部变量的函数
2. 也可以说是作用域应用的特殊情况
   1. 函数作为参数被传递
   2. 函数作为返回值

### 问：输入代码的结果

```javascript
// 函数作为返回值
function create() {
  let a = 100;
  return function () {
    console.log(a);
  };
}

let fn = create();
let a = 200;
fn();
```

```javascript
// 函数作为参数
function print() {
  let a = 200;
  fn();
}
let a = 100;
function fn() {
  console.log(a);
}
print(fn);
```

答：分别返回 100 和 100。自由变量的查找，是在函数定义的地方向上级作用域查找，不是在执行的地方

### 问：如何在不修改下面代码的情况下，修改 Obj 对象

```javascript
var o = (function () {
  var obj = {
    a: 1,
    b: 2,
  };
  return {
    get: function (k) {
      return obj[k];
    },
  };
})();
```

答：使用 `defineProperty`

```javascript
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get#defining_a_getter_on_existing_objects_using_defineproperty
Object.defineProperty(Object.prototype, "abc", {
  get() {
    return this;
  },
});

// abc 在 obj 上不存在，就会通过原型链往上找，找到的就是上面定义的 get 方法，返回的就是 obj 本身
let obj = o.get("abc");

obj.a = 3;
obj.c = 4;
```

### 问：面对上面的漏洞，如何预防

答：

1. 判断 k 为 obj 自有的属性

```javascript
return {
  get: function (k) {
    if (obj.hasOwnProperty(k)) {
      return obj[k];
    }
    return undefined;
  },
};
```

2. 如果不使用 `obj` 原型上的属性时，可以将原型设置为 `null`

```javascript
Object.setPrototypeOf(obj, null);
```

### 问：this 的不同应用场景，如何取值

答：

1. `this` 永远指向一个对象
2. `this` 的指向完全取决于函数调用的位置
3. 箭头函数的 `this` 指向其上下文中 `this` 的对象，在创建时就已经确定，不是在调用时确定

- 在普通函数中

```javascript
function fn() {
  let a = 100;
  console.log(this.a);
}

fn(); // undefined
```

- call, apply, bind 调用

```javascript
function fn() {
  let a = 100;
  console.log(this.a);
}

const fn1 = fn.bind({ a: 200 });

const res = fn1(); // 200
```
