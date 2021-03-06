# 我不知道的 Linux 小技巧

|    date    |    tag    |
|    ---     |    ---    |
| 2021-01-12 | linux/tip |

## 命令

- tail 监控重新创建的文件

    `tail -F file`

- watch 监控命令输出变化

- time 查看命令执行时间

- tmux 同步窗格

    命令行模式下输入 `:setw synchronize-panes` 可以同时在多个窗格输入命令

- sed 原文件执行快速备份

    `sed -i.bak 's/a/b' file` 会将原文件备份到 `file.bak`

- grep rg 查看过滤行的上下文

    `grep -A n` `grep -B n` `grep -C n` `rg -A -B -C`

- alias 别名快捷操作

    `L='| less'` `LL='2>&1 | less'` `NE='2> /dev/null'` `NUL='> /dev/null 2>&1'`

- seq 输出序列

    `seq` 组合 `xargs` 遍历指定次数, `seq 1000 | xargs -i dd if=/dev/zero of={} bs=1k count=256`

- 终端关闭声音

  - /etc/inputrc

    ```sh
    set set bell-style none
    ```

  - vim

    ```vim
    set visualbell
    set t_vb=
    ```

- 重新执行上次命令

    上次命令: `!!`
    上次参数: `alt + .`
