# 我不知道的 Linux 小技巧

| date       | tag       |
| ---------- | --------- |
| 2021-01-12 | linux/tip |

## other book

- [the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line)

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
    set bell-style none
    ```

  - vim

    ```vim
    set belloff=all
    # nvim don't have t_xx
    set t_vb=
    ```

- 重新执行上次命令

  上次命令: `!!`
  上次参数: `alt + .`

- shell 命令结果赋值给变量

  ```sh
  var=`command`
  var=$(command)
  ```

  两种方法推荐使用后者, 支持嵌套

- install 复制文件

  ```sh
  # 复制文件到指定目录(目录不存在时创建), 且设置权限
  install -Dm644 source_file dest_dir/dest_file
  ```

- 减少可执行文件体积

  - `strip` 可以剥掉文件的符号信息调试信息减少文件体积

  - `upx` 可进一步压缩可执行文件大小

- 查看进程的启动文件

  ```sh
  # 查询进程 pid
  ps -ef | grep [进程名]

  # 查看进程详细信息
  ls -l /proc/[pid]

  # exe 链接对应的就是可执行文件的路径
  ```

- 替换环境变量

  ```sh
  envsubst

  # Replace environment variables in stdin and output to stdout:

  echo '$HOME' | envsubst

  # Replace environment variables in an input file and output to stdout:

  envsubst < path/to/input_file

  # Replace environment variables in an input file and output to a file:

  envsubst < path/to/input_file > path/to/output_file

  # Replace environment variables in an input file from a space-separated list:

  envsubst '$USER $SHELL $HOME' < path/to/input_file

  ```

- 编辑管道 with $EDITOR

  ```sh
  echo foo | vipe | xargs echo
  ```

  [github](https://github.com/juliangruber/vipe)

  > moreutils

- 生成 temp 文件并用 $EDITOR 编辑

  ```sh
  # 生成临时文件
  mktemp
  # 生成临时文件并用编辑器编辑
  $EDITOR $(mktemp)
  ```

- bash 随机数

  ```sh
  # RANDOM shell 变量
  echo $RANDOM
  # $$ 变量返回当前进程号
  echo $$
  ```

- 模糊查询命令并执行

  ```sh
  $(history | fzf)
  ```

- print rest fields

  ```sh
  # in awk
  # Set the field(s) you want to skip to blank:
  awk '$1=""; print $0;' < file_name
  awk '$1=$2=""; print $0;' < file_name

  # in cut
  cut -d ' ' -f 2-
  cut -d \  -f 2-
  ```

  - [stackoverflow awk](https://stackoverflow.com/questions/18457486/print-rest-of-the-fields-in-awk)
  - [stackoverflow cut](https://stackoverflow.com/questions/2961635/using-awk-to-print-all-columns-from-the-nth-to-the-last/2961994#2961994)

- edit the current command line in an external editor

  bash: `C-x C-e`
  fish: `Alt-e` or `Alt-v`

- `locate`

  fast find file, recommand `plocate` (faster locate)

- `split` 及 `cat`

  `split` 可以对大文件进行切分, `cat` 可以将切分的多个文件合并

- 查看键编码

  `xev | awk -F'[ )]+' '/^KeyPress/ { a[NR+2] } NR in a { printf "%-3s %s\n", $5, $8 }'`

  `xmodmap -pke`

- 模拟按键调用功能

  通过 `xdotool` cli 程序可以方便的模拟按键或鼠标行为  
  调用按键对应的功能

- convert 切图

  ```sh
  convert -crop 100%x25% +repage input.png output.png
  ```

  > install: `sudo pacman -S imagemagick`

- `sudo`

  `sudo -E` 参数的作用是保留用户的环境变量来执行命令。
  `sudo` 默认会清除大部分环境变量,以提供较为干净的执行环境。
  但有时我们希望保留用户环境,这时可以用 `-E` 参数。

  另一个常用参数是 `-i`, 表示登录式的 `sudo`, 会模拟登录环境。

  两者区别在于:

  `-E` 只保留环境变量,不加载其他登录配置过程。
  `-i` 更完整的模拟交互式登录。

- append to pip, 追加内容到管道

  ```sh
  # 通过多个命令
  echo 'a' | (cat -; echo 'b')

  # 通过 cat 多个及输入重定向
  echo 'a' | cat - <(echo 'b')
  ```

- fold, 行宽度太长时换行

  `fold -w 80`

- paste, 按行拼接多个文件

  `paste $file1 $file2`

- head, 跳过末尾 n 行

  `head -n -5`: 输出除末尾 5 行的所有行, 可以理解为这里的 `-n` 为指定尾行为 `0-5`

- tail, 跳过前 n 行

  `tail -n +6`: 输出除前 5 行的所有行, 可以理解为这里的 `-n` 为指定首行为 `0+6`

- man file-hierarchy

  查看文件系统层次结构

- ssh 代理

  `~/.ssh/config`

  ```
  HOST *
      ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
  ```

  通过 openbsd-netcat(nc) socks5 代理

  > [原文](https://kuokuo.io/2019/07/01/ssh-over-http-or-socks/)

- q 一个使你的命令行输出拥有 sql 强大的动态查询能力

  > [https://github.com/harelba/q](https://github.com/harelba/q)

- awk 按列去重

  - `awk '!x[$2]++'`: 按第二列去重保留第一个
  - `awk '!int((x[$2]++)/2)'`: 按第二列去重, 保留两个

- grep / rg 精确匹配

  - `grep -F 'foo'`: 精确匹配 foo
  - `grep -F -e '-F'`: 使用 `-e` 避免将 `-F` 解析为选项
  - `grep -F -- -F`: 使用 `--` 避免将 `-F` 解析为选项

- grep / rg 输出匹配文件名及不匹配文件名

  - `grep -l '^\s'` | `rg -l '^\s'`: 输出包含匹配该模式的文件名
  - `grep -l -v '^\s'` | `rg -l -v '^\s'`: 输出包含不匹配该模式的行的文件名
  - `grep -L '^\s'` | `rg --files-without-match '^\s'`: 输出所有行都不匹配该模式的文件名

- 压缩多余空行

  ```sh
  cat -s
  ```

- sed 插入多行及行首空格及多个命令

  多个命令使用分号或回车分割, 简单命令用 `;`, 复杂用回车

  ```sh
  sed '2a\ second\
   third
  3d'
  ```

  ```
  sed '1a\hello;2d;3c\third'
  ```

- 其它命令的输出作为文件传递给其它命令, 进程替换(将命令的输出转换为一个匿名管道/文件, 或命名管道, 或临时文件)

  - bash
    `<(cmd)`
  - fish
    `(cmd | psub)`
    或者添加后缀, 作为 `C` 文件
    `(cmd | psub -s .C)`
