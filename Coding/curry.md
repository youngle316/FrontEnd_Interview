# 手写 curry 函数，实现函数柯里化

示例

```javascript
function add(a, b, c) {
  return a + b + c;
}

const curryAdd = curry(add);
curryAdd(10)(20)(30); // 60
```

实现 curry 这个函数

```typescript
function curry(fn: Function): Function {
  const fnArgsLength = fn.length;
  let args: any[] = [];

  function calc(...newArgs: any[]) {
    args = [...args, ...newArgs];
    if (args.length < fnArgsLength) {
      return calc;
    } else {
      return fn.apply(this, args.slice(0, fnArgsLength));
    }
  }

  return calc;
}
```
