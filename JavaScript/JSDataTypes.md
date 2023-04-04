# JS 基础-变量类型

### 问：`JS` 有哪些数据类型

答：基本数据类型 `Number`, `String`, `Boolean`, `Undefined`, `Null`, `BigInt`, `Symbol`。引用数据类型: `Object`

### 问：`typeof` 能判断哪些类型

答：
可以判断 `undefined`, `boolean`, `number`, `string`, `bigint`, `symbol`, `object` (包括 `null`),`function`

### 问：值类型与引用类型的区别

答：值类型存储在栈内存中，引用类型存储在堆内存中，赋值时值类型复制整个值，引用类型只复制地址。对值类型进行操作不会影响其他变量，而对引用类型的操作会影响所有引用该对象的变量。

### 问：什么是浅拷贝什么是深拷贝

答：浅拷贝只复制对象的一层属性，如果属性值是引用类型的话，则只复制它的引用，而不是完整的对象。也就是说，浅拷贝后的新对象和原对象共享同一个引用类型属性，对新对象进行修改可能会影响到原对象。深拷贝则是将整个对象及其所有子属性都完整复制一份，并创建一个新的对象。深拷贝后的对象和原对象互不影响，即使对新对象进行修改也不会影响原对象。

### 问：如何实现深拷贝

答：

- `JSON.Parse(JSON.Stringify)`
- 第三方库 `lodash`
- 手写实现

### 问：手写实现 `JS` 的深拷贝

答：

```javascript
function deepClone(obj = {}) {
  if (typeof obj !== "object" || obj == null) {
    return obj;
  }

  let result;
  if (obj instanceof Array) {
    result = [];
  } else {
    result = {};
  }

  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      result[key] = deepClone(obj[key]);
    }
  }

  return result;
}
```

### 问：判断类型都有哪些方法

答：

1. `typeof`
2. `instanceof`
3. `Object.prototype.toString().call()`
4. 其他如自带的 `API`，`Array.isArray`
