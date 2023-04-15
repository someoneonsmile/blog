# vim 技巧

| date       | tag        |
| ---------- | ---------- |
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

> note: map to '' in lua

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

## bufnr 0 in lua

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

### `set laststatus=3`: 全局状态栏

> 只展示最后一个窗口的状态栏

去除分隔背景, 减少分隔线宽度

```vim
hightlight WinSeparator guibg=None
```

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

## `verbose`: 查看定义

- `:verbose map <space>`: 查看所有映射
- `:verbose nmap <space>`: 查看 <space> 开头的 n 映射
- `:verbose set option_name`: option_name 的值, 及定义位置
- `:verbose function func_name`: 查看函数及定义位置
- `:verbose :command command_name`: 查看命令及定义位置
- `:verbose :[anything you can input in the ex]`: 查看命令及定义位置

> [stackexchange](https://vi.stackexchange.com/questions/21295/how-to-ask-vim-where-function-or-command-was-defined)

## `h: g`

vim 中以 `g` 开头的命令

```
tag		char	      note action in Normal mode	~
--------------------------------------------------------------------------------

|gstar|		g*		1  like "*", but without using "\<" and "\>"
|g#|		g#		1  like "#", but without using "\<" and "\>"

--------------------------------------------------------------------------------

|g&|		g&		2  repeat last ":s" on all lines

--------------------------------------------------------------------------------

|g'|		g'{mark}	1  like |'| but without changing the jumplist
|g`|		g`{mark}	1  like |`| but without changing the jumplist

--------------------------------------------------------------------------------

|g+|		g+		   go to newer text state N times
|g-|		g-		   go to older text state N times

|g,|		g,		1  go to N newer position in change list
|g;|		g;		1  go to N older position in change list

--------------------------------------------------------------------------------

|g?|		g?		2  Rot13 encoding operator
|g?g?|		g??		2  Rot13 encode current line
|g?g?|		g?g?		2  Rot13 encode current line

--------------------------------------------------------------------------------

|gJ|		gJ		2  join lines without inserting space

|gI|		gI		2  like "I", but always start in column 1
|gi|		gi		2  like "i", but first move to the |'^| mark

|gn|		gn	      1,2  find the next match with the last used
				   search pattern and Visually select it
|gN|		gN	      1,2  find the previous match with the last used
				   search pattern and Visually select it

|gp|		["x]gp		2  put the text [from register x] after the
				   cursor N times, leave the cursor after it
|gP|		["x]gP		2  put the text [from register x] before the
				   cursor N times, leave the cursor after it

--------------------------------------------------------------------------------

|g^|		g^		1  when 'wrap' off go to leftmost non-white
				   character of the current line that is on
				   the screen; when 'wrap' on go to the
				   leftmost non-white character of the current
				   screen line
|g0|		g0		1  when 'wrap' off go to leftmost character of
				   the current line that is on the screen;
				   when 'wrap' on go to the leftmost character
				   of the current screen line
|g_|		g_		1  cursor to the last CHAR N - 1 lines lower
|g$|		g$		1  when 'wrap' off go to rightmost character of
				   the current line that is on the screen;
				   when 'wrap' on go to the rightmost character
				   of the current screen line

|gm|		gm		1  go to character at middle of the screenline
|gM|		gM		1  go to character at middle of the text line

--------------------------------------------------------------------------------

|gg|		gg		1  cursor to line N, default first line

|gj|		gj		1  like "j", but when 'wrap' on go N screen
				   lines down
|gk|		gk		1  like "k", but when 'wrap' on go N screen
				   lines up

|go|		go		1  cursor to byte N in the buffer


--------------------------------------------------------------------------------

|gt|		gt		   go to the next tab page
|gT|		gT		   go to the previous tab page

--------------------------------------------------------------------------------

|gu|		gu{motion}	2  make Nmove text lowercase
|gU|		gU{motion}	2  make Nmove text uppercase
|g~|		g~{motion}	2  swap case for Nmove text

--------------------------------------------------------------------------------

|gv|		gv		   reselect the previous Visual area
|gV|		gV		   don't reselect the previous Visual area
				   when executing a mapping or menu in Select
				   mode

--------------------------------------------------------------------------------

|gq|		gq{motion}	2  format Nmove text
|gw|		gw{motion}	2  format Nmove text and keep cursor

|g@|		g@{motion}	   call 'operatorfunc'

--------------------------------------------------------------------------------
```

跳转到某行:

- `[count]G`
- `[count]gg`
- `:[count]`

## `h: opfunc`

`g@` 配合 `opfunc` 自定义操作符

ex: `surround.vim`  
ex: `easy_align.vim`

```
nmap ys function! s:opfunc() let &opfunc = 'some_func' return g@ endfunction

也可以在映射上直接拼接上 {motion}

nnoremap <expr>   <Plug>Yssurround '^'.v:count1.<SID>opfunc('setup').'g_'

```

> `g@`: call `opfunc`

## `h: z`

- `z=`: spell suggest

## Input Method

`:h iminsert`, `set iminsert=[0,1,2]`

- 0: 禁用输入法自动切换
- 1: lmap is ON and IM is off, `:h lmap`: for insert/cmd/search mode map with auto switch im
- 2: 启用输入法自动切换, lmap is off and IM is ON

同时会影响

- `imsearch`: 搜索模式输入法自动切换, `imcmdline`: 命令行输入法自动切换, 不受
  `iminsert` 影响

- `CTRL-^`: edit the alternative file, 编辑(切换文件)

`:h im-server`

## keymap

- `!`, `:map!`: insert and command line mode

- `o`, `:omap`: operator pending mode

- `''`, `:map`: normal visual select and operator-pending

- `v`, `:vmap`: visual and select, ex: vnoremap <silent> F :<C-U>ggVG<CR>, 类似于可视模式下的操作符

### 取消 keymap

- `unmap key`: in vim

- `vim.keymap.del`: in lua

## 操作符, 文本对象

- [ ] opfunc defined, use g@ and opfunc

- [ ] textobj defined, use the operator-pending map, maybe it actually same as visual, pending 等待选区

      ```viml
      for [s:k, s:m] in  [['ie', 'ggVG'], ['il', '^vg_'], ['iz', '[zV]z']]
          execute 'onoremap <silent> ' . s:k . ' :normal! ' . s:m . '<CR>'
          execute 'vnoremap <silent> ' . s:k . ' :normal! ' . s:m . '<CR>'
      endfor
      unlet! s:k s:m
      ```
      [reddit](https://www.reddit.com/r/vim/comments/48e4ci/vimscript_how_to_create_a_new_text_object/)

## program

- `=`: `equalprg`, `indentexpr`
- `gq` `gw`: `formatexpr`, `formatprg`

- `grepprg`: `grep`
- `makeprg`: `make`

## 模拟输入

- `nvim_feedkeys`

阻塞的方式模拟用户输入

- `nvim_input`

低级 Api, 以非阻塞方式运行

## jump in help

在帮助文件中跳转的两种方式

- `K`: `<shift+k>`
- `<C-]`

## wincmd

`:[count]winc[md] {arg}`: Like executing CTRL-W [count] {arg}.  
Example: `:wincmd j`

## defaulttable

`vim.defaulttable`

```lua
local a = vim.defaulttable()
a.b.c = 1
```
