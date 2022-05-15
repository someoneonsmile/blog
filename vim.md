# vim 技巧

|    date    |    tag     |
|    ---     |    ---     |
| 2020-11-05 | vim/repeat |

## vim 重复输入

自动输入指定个数的重复数字， 例如: `10a=`, `2yl`, `2yy`, `2yw`

## `<c-r>` 快捷键

`normal` 模式下为重做修改的内容

`insert` 模式为输出寄存器中的内容

- `<c-r><c-w>`: 命令行下输入光标下单词

- `<c-r>/`: 输入上次查找的内容

- `<c-r>:`: 输入上次执行的 `Ex` 命令

- `<c-r>*`: 输入寄存器中的内容

- `<c-r>=`: 输入表达式寄存器中的内容

## 查找替换

`:s//otherthing`: `//` 查找域留空会重用上次的查找模式

`:s//\=@*`: `\=@*` 会引用寄存器中的内容. 重用上次的查找, 并替换为剪贴板中的内容

`:s//\=submatch(0)-1/g`: `\=` 可以执行 `vim` 脚本表达式

`:s//~/`: 使用上次查找模式, 使用上次的替换字符, 不记住标志位

`:s//~/&`: 使用上次查找模式, 使用上次的替换字符, 使用上次标志位

`&`: 在当前行执行上次的替换命令, 相当于 `:s//~/`, 相同的查找模式, 相同的替换字符, 不记住标志位

`g&`: 在所有行执行上次的替换命令, 相当于 `:%s//~/&`, 相同的查找模式, 相同的替换字符, 相同的标志位

`:&&`: 在当前行上执行上次的替换, 第一个 `&` 作为 `Ex` 命令的组成部分, 第二个 `&` 会重用上次 `:s` 的标志位

`:%S/{man,dog}/{dog,man}/g`, `Abolish.vim` 插件, 可轻松实现单词交换

`/nostart \zs pattern \ze noend`: `\zs` 用于指定匹配开始边界, `\ze` 用于指定匹配结束边界

`:%s//~/gc`: `c` 标识符会在每次替换前进行确认

## global 命令

`global` 可对匹配的所有行执行操作, 可配合 `normal` 命令

`:g/^reg$/normal .`

`:vglobal`(`:v`), `:global!`(`:g!`) 可对不匹配的所有行执行操作

`:g/{/ .+1,/}/-1 sort`: 可对 `{}` 内的文本行按照字母顺序重新排列, `.+1` 跟 `/}/-1` 是命令 sort 的范围

`global` 命令的标准格式如下:

`:g/{pattern}/[cmd]`, 牢记 `Ex` 命令通常都会接收 "范围" 作为其参数

`:global` 命令内容的 `[cmd]`, 该规则依然有效

`:g/{pattern}/[range][cmd]`

## `:t` `:m` 复制和移动行

结合 `global` 命令可将匹配的行快速的移动或复制

`:m .-2` 当前行与上行交换
`:m .+1` 当前行与下行交换

## `o` 切换高亮选区的活动端

## 不重启 `vim` 的情况下，重载配置文件

`:so %` or `:so ~/.vimrc`

## `set paste` 设置粘贴模式

会影响插入模式的快捷键

## 设置错误提示侧边拦宽度

`set signcolumn=yes:1`

## vim ctrl-s 卡死

`<C-s>`: 锁定屏幕, linux 终端让屏幕暂停输入, 键盘仍然可以使用, 非 vim 问题
`<C-q>`: 解锁屏幕

## 禁用 keymap

`map to <NOP>`

## 查看特殊键码

`:h key-codes`

## vim 配置文件结构

- device.vim
- gui.vim
- set.vim
- keymap.vim
- command.vim
- autocommand.vim
- function.vim
- plugin.vim

## bufnr 0

> `bufnr: 0` mean current buf
> `window` `tab` 也是同样

## :执行 lua 函数的方式

- `:lua func_name`
- `:v:lua.func_name`

## vim ^= += -=

- `+=`: 在之后追加
- `^=`: 在之前追加
- `-=`: 减少

> lua api 也提供了 `^` `+` `-` 符号操作
> ex: `vim.opt.clipboard = vim.opt.clipboard ^ 'unnamedplus'`

## nvim-0.7

### `:lua =`

> `:lua =`: since nvim-0.7, pretty_print the lua express

### `set laststatus=3`

```vim
hightlight WinSeparator guibg=None
```
> 只展示最后一个窗口的状态栏


## `:=`

- `:=`: print the last line num
- `{range}:=`: print the last line num in {range}
    ex: `:.=` print the current line num

## `:!!`

> repeat laste `:!{cmd}`


## `q:`

> 打开命令行历史窗口

## `:filter`

> `:h filter`

`filter`: 过滤命令的输出

## lua 捕获 EX 命令输出

`vim.api.nvim_exec`

```lua
local output = vim.api.nvim_exec([[map]], true)
print(output)
```
