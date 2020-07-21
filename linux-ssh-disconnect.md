# linux ssh 无响应时怎么退出

ssh 无响应时, `<C-c>` `<C-d>` 等都退出不了

搜索时发现可以用 `~.` 优雅的断开连接

```sh
~.
```

man 帮助文件显示如下：

```sh
$ man ssh
...
ESCAPE CHARACTERS
     ...

     The supported escapes (assuming the default `~') are:

     ~.      Disconnect.
```
