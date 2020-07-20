# linux json 处理工具

> 2020-07-20

## 官网介绍

> jq is like `sed` for JSON data  
> you can use it to slice and filter and map and transform structured data  
> with the same ease that `sed`, `awk`, `grep` and friends let you play with text.

[官网](https://stedolan.github.io/jq/)

## 用法

`sf` 代表某字段(someField), `on` 代表别名(othername)

- `cat some.json | jq '.'` : 显示全部 json 内容

- `cat some.json | jq '{sf1, sf2}'` : 显示部分 json 字段

- `cat some.json | jq '{sf1, othername: .sf2}'` : 别名显示

- `cat some.json | jq '.[] | {sf1, sf2}'` : `|` 数组的每一项都处理一次, 显示为 `{}{}`

- `cat some.json | jq '.[0] | {sf1, sf2}'` : 数组的第一项

- `cat some.json | jq '[.[] | {sf1, sf2}]'` : 数组的处理结果放入数组中, 显示为 `[{}{}]`

- `cat some.json | jq '{sf1, othername: {sf2, sf3}}'` : 嵌套对象

- `cat some.json | jq '[.[] | {sf1, [.sf2[].sf3]}]'` : 嵌套数组

- `cat some.json | jq '[.[] | {sf1, on1: [{on2: .sf2[].sf3}]}]'` : 嵌套数组对象

- `cat some.json | jq '[.[] | {sf1, on1: [.sf2[] | {on2: .sf3}]}]'` : 嵌套 `|`

## 总结

- `.` 代表根

- `|` 代表对数组的每一项都进行操作

- `{}` 代表将结果组为对象

- `[]` 代表将结果组为数组

- `sf` 名称跟值同时取, 相当于 `sf: .sf`

- `|` 嵌套管道后的 `.` 代表最近管道根对象, 就是说 `|` 会改变 `.` 根对象

