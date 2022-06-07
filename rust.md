# Rust Tips

| date       | tag         |
| ---------- | ----------- |
| 2022-06-06 | rust / tips |

## match pattern to bool

- `matches!`: convert the match pattern to bool

- `cfg!`: convert the cfg macro to bool

## return 和省略 return ; 区别

return: 用于突破嵌套块范围, 提前返回给函数结果 (break out of any amount of nested scopes until it hits a function-like scope)

省略分号: 作为最后一个表达式, 所以只会返回到最近的 block `{}`

分号: 用于分隔表达式

return statement also has a type of its own, that is to say that `let x = return;` will actually compile.

### souce

- [stackoverflow](https://stackoverflow.com/questions/59013389/whats-the-difference-between-using-the-return-statement-and-omitting-the-semico)

- [rust user](https://users.rust-lang.org/t/omitting-the-return-keyword/29655/2)
