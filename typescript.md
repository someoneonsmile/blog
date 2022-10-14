# typescript 浅识

| date       | tag |
| ---------- | --- |
| 2022-10-13 | ts  |

## tips

- 从联合类型中去除 null 或者 undefined

  `NonNullable`, ex:`type NonNullType = NonNullable<NullType>`

  [dev.to](https://dev.to/vborodulin/ts-how-to-override-properties-with-type-intersection-554l)
  [typescriptlang utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

- 覆盖类型

  `type Modify<T, R> = Omit<T, keyof R> & R`

  [stackoverflow](https://stackoverflow.com/questions/41285211/overriding-interface-property-type-defined-in-typescript-d-ts-file#55032655)

- 获取属性的类型

  `type attrType = InterfaceType['attr']`

- `TS` 工具类型

  [typescriptlang](https://www.typescriptlang.org/docs/handbook/utility-types.html)

- `readonly` 类型转为 `mutable`

  [blog](https://bobbyhadz.com/blog/typescript-change-readonly-to-mutable)
