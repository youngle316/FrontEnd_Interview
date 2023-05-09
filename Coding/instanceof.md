# instanceof 原理是什么，请用代码表示

```javascript
const a = [];
a instanceof Array;
```

instanceof 的原理通过原型链，判断 `a.__proto__ === Array.prototype`，如果不等于则继续通过原型链继续向上找直到找到返回 true 或返回 false

```typescript
const myInstanceof = (instance, fn) => {
  let proto = instance.__proto__;
  while (proto) {
    if (proto === fn.prototype) {
      return true;
    } else {
      proto = proto.__proto__;
    }
  }
  return false;
};
```
