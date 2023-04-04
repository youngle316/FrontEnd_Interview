# 如何理解 HTML 语义化

### 问：`HTML` 语义化有什么优点

```html
<div>标题</div>
<div>
  <div>一段文字</div>
  <div>
    <div>列表一</div>
    <div>列表二</div>
  </div>
</div>
```

```html
<h1>标题</h1>
<div>
  <p>一段文字</p>
  <ul>
    <li>列表一</li>
    <li>列表二</li>
  </ul>
</div>
```

通过上面代码的比较，语义化有以下两个优点

- 让人更容易读懂（增加代码可读性）
- 让搜索引擎更容易读懂（更好的 `SEO`）
