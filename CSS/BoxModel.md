# 盒模型宽度计算

```html
<style>
  #div1 {
    width: 100px;
    padding: 10px;
    border: 1px solid #ccc;
    margin: 10px;
  }
</style>

<div id="div1">box</div>
```

### 问：`div1` 的 `offsetWidth` 是多少

答：`offsetWidth` 包含了 `content` + `border` + `padding`，所以为 122

### 问：如果要使 `div1` 的 `offsetWidth` 为 `100px`，该如何实现

答：设置 `box-sizing: border-box`
