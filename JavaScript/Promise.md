# JS 进阶-异步

### 问：同步和异步的区别是什么

答：同步操作是按照代码书写的顺序依次执行，而异步操作则可以先执行后面的代码，等待异步操作完成后再执行相应的回调函数或 `Promise` 的 `then` 方法。

### 问：说出下面代码的执行顺序

```javascript
console.log(1);

setTimeout(() => {
  console.log(2);
}, 1000);

console.log(3);

setTimeout(() => {
  console.log(4);
});

console.log(5);
```

答：1 - 3 - 5 - 4 - 2

### 问：请描述 event loop 的机制，可画图

答：

1. 同步任务，一行一行的放在 Call Stack 执行
2. 遇到异步，会先记录下，等待时机（定时或网络请求）
3. 时机到了，就移动到 Callback Queue
4. 如 Call Stack 为空（即同步代码执行完了）Event Loop 开始工作
5. 轮训查找 Callback Queue，如有则移动到 Call Stack 执行
6. 然后继续轮训查找

### 问：简单介绍一下 Promise

答：`Promise` 是异步编程的解决方式，它可以避免回调函数嵌套过深的问题，并且可以很好的处理异步操作的结果。他有三种状态，分别为 `pending`，`fulfilled`，`rejected`，当异步操作完成后状态会从 `pending` 转为 `fulfilled` 或 `rejected`，并且是不可逆的。并且可以通过 `then`，`catch` 方法获取到异步操作的结果或错误信息

### 问：说出下面代码的执行结果

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
  })
  .catch(() => {
    console.log(2);
  })
  .then(() => {
    console.log(3);
  });
```

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
    throw new Error("error");
  })
  .catch(() => {
    console.log(2);
  })
  .then(() => {
    console.log(3);
  });
```

```javascript
Promise.resolve()
  .then(() => {
    console.log(1);
    throw new Error("error");
  })
  .catch(() => {
    console.log(2);
  })
  .catch(() => {
    console.log(3);
  });
```

答：上面三段代码答案分别为`1 - 3`，`1 - 2 - 3`，`1 - 2`

### 问：async/await 和 Promise 的关系

答：

1. 执行 `async` 函数，返回的是 `Promise` 对象
2. `await` 相当于 `Promise` 的 `then`
3. `try catch` 可捕获异常，替代了 `Promise` 中的 `catch`

### 问：说出下面代码的执行结果

```javascript
(async function () {
  console.log("start");
  const a = await 100;
  console.log("a", a);
  const b = await Promise.resolve(200);
  console.log("b", b);
  const c = await Promise.reject(300);
  console.log("c", c);
  console.log("end");
})();
```

答：start -> 'a': 100 -> 'b': 200 。 后面报错不会打印

### 问：什么是宏任务，什么是微任务，它们的区别是什么

答：

1. 宏任务：`setTimeout`，`setInterval`，`ajax` 请求，`DOM` 事件
2. 微任务：`Promise` 的 `then` 方法，`async/await`
3. 微任务比宏任务要优先执行

### 问：为什么微任务比宏任务执行更早

答：因为微任务的执行时机在当前任务执行结束之后，而宏任务则需要等待当前任务及其所有微任务执行完成后才能执行。这意味着，无论何时有微任务被添加到任务队列中，它们都会在下一个事件循环周期中被立即执行，而不需要等待其他任务的完成。

### 问：说出下面代码的执行结果

```javascript
console.log(100);
setTimeout(() => {
  console.log(200);
});
Promise.resolve().then(() => {
  console.log(300);
});
console.log(400);
```

答：100 -> 400 -> 300 -> 200

### 问：说出下面代码的执行结果

```javascript
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}
console.log("script start");

setTimeout(() => {
  console.log("setTimeout");
}, 0);

async1();
new Promise((resolve) => {
  console.log("promise1");
  resolve();
}).then(() => {
  console.log("promise2");
});

console.log("script end");
```

答：script start -> async1 start -> async2 -> promise1 -> script end -> async1 end -> promise2 -> setTimeout

### 问：手写一个 Promise

```javascript
class MyPromise {
  state = "pending";
  value = undefined;
  reason = undefined;

  resolveCallbacks = [];
  rejectCallbacks = [];

  constructor(fn) {
    const resolveHandler = (value) => {
      if (this.state === "pending") {
        this.state = "fulfilled";
        this.value = value;
        this.resolveCallbacks.forEach((fn) => fn(this.value));
      }
    };
    const rejectHandler = (reason) => {
      if (this.state === "pending") {
        this.state = "rejected";
        this.reason = reason;
        this.resolveCallbacks.forEach((fn) => fn(this.reason));
      }
    };

    try {
      fn(resolveHandler, rejectHandler);
    } catch (error) {
      rejectHandler(error);
    }
  }

  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === "function" ? onFulfilled : (v) => v;
    onRejected =
      typeof onRejected === "function"
        ? onRejected
        : (e) => {
            throw e;
          };

    if (this.state === "pending") {
      return new MyPromise((resolve, reject) => {
        this.resolveCallbacks.push((value) => {
          try {
            const x = onFulfilled(value);
            resolve(x);
          } catch (error) {
            reject(error);
          }
        });
        this.rejectCallbacks.push((reason) => {
          try {
            const x = onRejected(reason);
            resolve(x);
          } catch (error) {
            reject(error);
          }
        });
      });
    }

    if (this.state === "fulfilled") {
      return new MyPromise((resolve, reject) => {
        try {
          const x = onFulfilled(this.value);
          resolve(x);
        } catch (error) {
          reject(error);
        }
      });
    }

    if (this.state === "rejected") {
      return new MyPromise((resolve, reject) => {
        try {
          const x = onRejected(this.reason);
          resolve(x);
        } catch (error) {
          reject(error);
        }
      });
    }
  }

  catch(onRejected) {
    return this.then(null, onRejected);
  }
}

MyPromise.resolve = function (value) {
  return new MyPromise((resolve) => resolve(value));
};

MyPromise.reject = function (reason) {
  return new MyPromise((resolve, reject) => reject(reason));
};

MyPromise.all = function (promises) {
  return new MyPromise((resolve, reject) => {
    const result = [];
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then((data) => {
        result[i] = data;
        count++;
        if (count === promises.length) {
          resolve(result);
        }
      }, reject);
    }
  });
};

MyPromise.race = function (promises) {
  return new MyPromise((resolve, reject) => {
    for (let i = 0; i < promises.length; i++) {
      promises[i].then(resolve, reject);
    }
  });
};
```
