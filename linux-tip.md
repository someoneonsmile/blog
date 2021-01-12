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
