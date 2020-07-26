# vim 中将 `tab` 自动转换成空格

在Vim中，有时需要将tab转换成space。使用ret命令（replace tab）

> `[range]ret[ab]! [new-tabstop]`

举例：将第一行到文件尾的tab转换成space，每个tab用4个space替代。

```vim
:set expandtab
:%ret! 4
```

如果没有给定4，则用当前的tab宽度设定替换为space

其它相关命令：

- `:set tabstop=4` 设定tab宽度为4个字符

- `:set shiftwidth=4` 设定自动缩进为4个字符

- `:set expandtab` 用space替代tab的输入

- `:set noexpandtab` 不用space替代tab的输入

这种设置只是针对当前的文件。如果想让设置对所有的文件都有效，可以修改Vim的配置文件.vimrc，将设置命令添加到文件中。
