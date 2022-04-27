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

- shell 命令结果赋值给变量

    ```shell
    var=`command`
    var=$(command)
    ```

    两种方法推荐使用后者, 支持嵌套

- install 复制文件

    ```shell
    # 复制文件到指定目录(目录不存在时创建), 且设置权限
    install -Dm644 source_file dest_dir/dest_file
    ```
- 减少可执行文件体积

    - `strip` 可以剥掉文件的符号信息调试信息减少文件体积

    - `upx` 可进一步压缩可执行文件大小

- 查看进程的启动文件

    ```shell
    # 查询进程 pid
    ps -ef | grep [进程名]

    # 查看进程详细信息
    ls -l /proc/[pid]

    # exe 链接对应的就是可执行文件的路径
    ```
