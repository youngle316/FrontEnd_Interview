# new 运算符的过程是什么，手写一个表示

1. 创建一个空对象，空对象的 `__proto__` 指向构造函数的原型对象
2. 执行构造函数，将构造函数的 this 指向改为创建的对象
3. 如果构造函数又返回值，并且返回值的类型是 object，则返回构造函数的返回值
4. 否则返回创建的实例对象

```typescript
const myNew = (fn: Function, ...args: any) => {
  const instance = Object.create(fn.prototype);
  const result = fn.apply(instance, args);
  if (result && typeof result === "object" && result !== null) {
    return result;
  }
  return instance;
};
```
