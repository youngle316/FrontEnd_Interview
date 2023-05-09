# 手写一个 LazyMan，实现 sleep 机制

实现 eat 和 sleep 方法

示例：

```javascript
const me = new LazyMan("le");
me.eat("苹果").eat("香蕉").sleep(5).eat("葡萄");

// le eat 苹果
// le eat 香蕉
// le sleep 5s  -- 5秒后再执行
// le eat 葡萄
```

```typescript
class LazyMan {
  private name: string = "";
  private tasks: Function[] = [];

  constructor(name: string) {
    this.name = name;
    setTimeout(() => {
      this.next();
    }, 0);
  }

  private next() {
    const task = this.tasks.shift();
    task && task();
  }

  eat(food: string) {
    const task = () => {
      console.log(`${this.name} eat ${food}`);
      this.next();
    };
    this.tasks.push(task);
    return this;
  }

  sleep(seconds: number) {
    const task = () => {
      setTimeout(() => {
        console.log(`${this.name} sleep ${seconds}s`);
        this.next();
      }, seconds * 1000);
    };
    this.tasks.push(task);
    return this;
  }
}
```
