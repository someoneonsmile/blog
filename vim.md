# vim 技巧

|    date    |    tag     |
|    ---     |    ---     |
| 2020-11-05 | vim/repeat |

## vim 重复输入

自动输入指定个数的重复数字， 例如: `10a=`, `2yl`, `2yy`, `2yw`

## 命令行下输入光标下单词

`<c-r><c-w>`

## global 命令

global 可对匹配的所有行执行操作, 可配合 normal 命令

> `:g/^reg$/normal .`
> `:g/^reg$/normal .`

## 不重启 `vim` 的情况下，重载配置文件

`:so %` or `:so ~/.vimrc`
