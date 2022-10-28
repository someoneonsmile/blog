# React 浅识

| date       | tag   |
| ---------- | ----- |
| 2022-10-13 | react |

## tips

### `Provider` 模式

借助 `Context` 以及自定义 `Hook` 实现命令式 `API`

> qsa-todo

- [global dialog](https://github.com/sheason2019/global-dialog-demo)
- [命令式 Dialog](https://cache.one/read/16377313)

### `key`: 没有 ID 的内部列表 key 怎么设置

- 外部 id 作为前缀 + index
- 内容经 objectHash 处理后作为 key (只读时可用，编辑时会有每次输入都失去焦点的 bug)

### 多个滚动条同步滚动

- [原生 JS 控制多个滚动条同步跟随滚动](https://juejin.cn/post/6844903539353845767)
- [原生 JS 解决多节点滚动同步联动](https://juejin.cn/post/6844904020281147405)
