# vim 中 tab 相关设置

### vim 中将 `tab` 自动转换成空格

在 `Vim` 中，有时需要将 `tab` 转换成 `space`。使用 `expandtab` 并结合 `ret` 命令（`retab`）

> `[range]ret[ab]! [new-tabstop]`
> 文件重新设置 `tab`, 并尊重 `expandtab` 设置, 并设置新 `tabstop`
> 新设置的 `tabstop` 不对当前 `retab` 生效

举例：将第一行到文件尾的tab转换成space，每个tab用4个space替代。

```vim
:set expandtab
:set tablestop=4
" :%ret! 4 " 默认 range 为整个文件
:ret!
```

其它相关命令：

- `:set tabstop=4` 设定tab宽度为 4 个字符

- `:set shiftwidth=4` 设定自动缩进为 4 个字符

- `:set expandtab` 用 `space` 替代 `tab` 的输入

- `:set noexpandtab` 不用 `space` 替代 `tab` 的输入

这种设置只是针对当前的文件。如果想让设置对所有的文件都有效，可以修改 `Vim` 的配置文件 `.vimrc`，将设置命令添加到文件中。


### 符号形式显示 tab

```vim
" 设置 tab 符号
set listchars=tab:> ,trail:-,nbsp:+
" 符号形式显示 tab
set list
```
