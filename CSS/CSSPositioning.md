# CSS 定位

### 问：`absolute` 和 `relative` 分别依据什么定位

答：

`relative` 依据自身定位

`absolute` 依据最近一层的定位元素定位（定位元素：`relative`、`fixed`、`absolute`、`body`）

### 问：居中对齐的方式有哪些

答：

`flex` 布局 `justify-content: center`, `align-items: center`

水平居中：

- `inline` 元素：`text-align: center`
- `block` 元素：`margin: auto`
- `absolute` 元素：`left: 50%` + `margin-left` 负值

垂直居中：

- `inline` 元素：`line-height` 的值等于 `height`
- `absolute` 元素：`top： 50%` + `margin-top` 负值
- `absolute` 元素：`transform(-50%, -50%)`
- `absolute` 元素：`top, left, bottom, right` 等于 `0 + margin: auto`
