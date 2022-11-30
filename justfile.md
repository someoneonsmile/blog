# just

`just` is a handy way to save and run project-specific commands.

## 语法特性

- 变量

```
var_name := var_value
```

ex:

```
localhost := `dumpinterfaces | cut -d: -f2 | sed 's/\/.*//' | sed 's/ //g'`
```

- 导出环境变量

```
export RUST_BACKTRACE := "1"

test:
  # 如果它崩溃了, 将打印一个堆栈追踪
  cargo test
```

以 `$` 为前缀的参数将被作为环境变量导出:

```
test $RUST_BACKTRACE="1":
  # 如果它崩溃了, 将打印一个堆栈追踪
  cargo test
```

导出的变量和参数不会被导出到同一作用域内反引号包裹的表达式里.

```
export WORLD := "world"
# This backtick will fail with "WORLD: unbound variable"
BAR := `echo hello $WORLD`
```

```
# Running `just a foo` will fail with "A: unbound variable"
a $A $B=`echo $A`:
  echo $A $B
```

当 `export` 被设置时, 所有的 `just` 变量都将作为环境变量被导出.

- 从 `.env` 文件加载环境变量

如果 `dotenv-load` 被设置, `just` 将从一个名为 `.env` 的文件中加载环境变量.
这个文件可以和你的 `justfile` 位于同一目录下, 或者位于其父目录下.
这些变量是环境变量, 而不是 `just` 的变量, 因此必须使用 `$VARIABLE_NAME` 在配方和反引号中访问

例如, 假如你的 `.env` 文件包含:

```
# 注释, 将被忽略
DATABASE_ADDRESS=localhost:6379
SERVER_PORT=1337
```

而你的 `justfile` 包含:

```
set dotenv-load

serve:
  @echo "Starting server with database $DATABASE_ADDRESS on port $SERVER_PORT…"
  ./server --database $DATABASE_ADDRESS --port $SERVER_PORT
```

`just serve` 将会输出:

```
$ just serve
Starting server with database localhost:6379 on port 1337…
./server --database $DATABASE_ADDRESS --port $SERVER_PORT
```

- 别名

```
alias alias_name = recipe_name
```

作用: 为配方取别名, 可以使用配方名称更简短

> note: `recipe` (配方)

- 反引号命令求值

```
localhost := `dumpinterfaces | cut -d: -f2 | sed 's/\/.*//' | sed 's/ //g'`
```

反引号可以用来存储命令的求值结果

````
# This backtick evaluates the command \`echo foo\necho bar\n\`, which produces the value \`foo\nbar\n\`.
stuff := ```
  echo foo
  echo bar
```
````

三个反引号, 同多行缩进字符串, 会删除所有行共同的缩进

- 条件表达式

`if` / `else`

```
foo := if env_var("RELEASE") == "true" { `get-something-from-release-database` } else { "dummy-value" }
```

多个 `if` / `else` 可以连起来使用:

```
foo := if "hello" == "hi" {
  "xyz"
} else if "a" == "a" {
  "abc"
} else {
  "123"
}
```

可以用 error 函数停止执行. 比如:

```
foo := if "hello" == "hi" {
  "xyz"
} else if "a" == "a" {
  error("abc")
} else {
  "123"
}
```

条件语句也可以在配方中使用:

```
bar foo:
  echo {{ if foo == "bar" { "hello" } else { "goodbye" } }}
```

> note: 注意最后的 `}` 后面的空格! 没有这个空格, 插值将被提前结束.

- 逻辑判断运算符

  - `==`: 判断相等
  - `!=`: 判断不相等
  - `=~`: 正则表达式进行匹配 (由 [Regex 包](https://github.com/rust-lang/regex)提供)

- 设置

设置控制解释和执行. 每个设置最多可以指定一次, 可以出现在 justfile 的任何地方.

例如:

```
set shell := ["zsh", "-cu"]

foo:
  # this line will be run as `zsh -cu 'ls **/*.txt'`
  ls **/*.txt
```

- `shell`
- `export`
- `allow-duplicate-recipes`
- `positional-arguments`
- `dotenv-load`

[设置一览表](https://github.com/casey/just/blob/master/README.%E4%B8%AD%E6%96%87.md#%E8%AE%BE%E7%BD%AE%E4%B8%80%E8%A7%88%E8%A1%A8)

- 变量和替换

`{{...}}` 中的表达式会在脚本运行前被 `just` 替换

支持在变量、字符串、拼接、路径连接和替换中使用 `{{…}}` :

变量支持声明变量及配方参数变量

```
tmpdir  := `mktemp`
version := "0.2.7"
tardir  := tmpdir / "awesomesauce-" + version
tarball := tardir + ".tar.gz"

publish:
  rm -f {{tarball}}
  mkdir {{tardir}}
  cp README.md *.c {{tardir}}
  tar zcvf {{tarball}} {{tardir}}
  scp {{tarball}} me@server.com:release/
  rm -rf {{tarball}} {{tardir}}
```

- 字符串

双引号字符串支持转义序列:

```
string-with-tab             := "\t"
string-with-newline         := "\n"
string-with-carriage-return := "\r"
string-with-double-quote    := "\""
string-with-slash           := "\\"
string-with-no-newline      := "\
"
```

单引号字符串不支持转义序列:

```
escapes := '\t\n\r\"\\'
```

```
$ just --evaluate
escapes := "\t\n\r\"\\"
```

字符串可以包含换行符:

```
single := '
hello
'

double := "
goodbye
"
```

支持单引号和双引号字符串的缩进版本, 以三个单引号或三个双引号为界. 缩进的字符串行被删除了所有非空行所共有的前导空白:

```
# 这个字符串执行结果为 `foo\nbar\n`
x := '''
  foo
  bar
'''

# 这个字符串执行结果为 `abc\n  wuv\nbar\n`
y := """
  abc
    wuv
  xyz
"""
```

- 函数

[doc](https://github.com/casey/just/blob/master/README.%E4%B8%AD%E6%96%87.md#%E5%87%BD%E6%95%B0)

## 配方

- 默认配方

当 `just` 被调用而没有传入任何配方时, 它会运行 `justfile` 中的第一个配方. 这个配方可能是项目中最常运行的命令, 比如运行测试:

```
test:
  cargo test
```

你也可以使用依赖关系来默认运行多个配方:

```
default: lint build test

build:
  echo Building…

test:
  echo Testing…

lint:
  echo Linting…
```

在没有合适配方作为默认配方的情况下, 你也可以在 justfile 的开头添加一个配方, 用于列出可用的配方:

```
default:
  just --list
```

- 私有配方

名字以 `_` 开头的配方和别名将在 `just --list` 中被忽略:

```
test: _test-helper
  ./bin/test

_test-helper:
  ./bin/super-secret-test-helper-stuff
```

`[private]` 属性也可用于隐藏配方, 而不需要改变名称:

```
[private]
foo:

bar:
```

这对那些只作为其他配方的依赖使用的辅助配方很有用.

- 安静配方

配方名称可在前面加上 `@`, 可以在每行反转行首 `@` 的含义:

```
@quiet:
  echo hello
  echo goodbye
  @# all done!
```

现在只有以 `@` 开头的行才会被回显:

```
$ just quiet
hello
goodbye
# all done!
```

`Shebang` 配方默认是安静的:

```
foo:
  #!/usr/bin/env bash
  echo 'Foo!'
```

```
$ just foo
Foo!
```

在 `Shebang` 配方名称前面添加 `@`, 使 `just` 在执行配方前打印该配方:

```
@bar:
  #!/usr/bin/env bash
  echo 'Bar!'
```

```
$ just bar
#!/usr/bin/env bash
echo 'Bar!'
Bar!
```

`just` 在配方行失败时通常会打印错误信息, 这些错误信息可以通过 `[no-exit-message]` 属性来抑制. 你可能会发现这在包装工具的配方中特别有用:

```
git *args:
    @git {{args}}
```

```
$ just git status
fatal: not a git repository (or any of the parent directories): .git
error: Recipe `git` failed on line 2 with exit code 128
```

添加属性, 当工具以非零代码退出时抑制退出错误信息:

```
[no-exit-message]
git *args:
    @git {{args}}
```

```
$ just git status
fatal: not a git repository (or any of the parent directories): .git
```

- 配方属性

| 名称                | 描述                               |
| ------------------- | ---------------------------------- |
| `[no-cd]`           | 在执行配方之前不要改变目录         |
| `[no-exit-message]` | 如果配方执行失败, 不要打印错误信息 |
| `[linux]`           | 在 `Linux` 上启用配方              |
| `[macos]`           | 在 `Macos` 上启用配方              |
| `[unix]`            | 在 `Unixes` 上启用配方             |
| `[windows]`         | 在 `Windows` 上启用配方            |

`[linux]`, `[macos]`, `[unix]` 和 `[windows]` 属性是配置属性.
默认情况下, 配方总是被启用.
一个带有一个或多个配置属性的配方
只有在其中一个或多个配置处于激活状态时才会被启用.

- 配方文档

配方上的注释会出现在 `just --list`

- 配方依赖

配方的多个依赖没有相互依赖关系时, 按声明顺序执行

```
@run: a b c
  echo run

@a:
  echo a

@b:
  echo b

@c:
  echo c
```

```
$ just run
a
b
c
run
```

后置依赖:

一个配方的正常依赖总是在配方开始之前运行. 也就是说, 被依赖方总是在依赖方之前运行. 这些依赖被称为 "前期依赖".

一个配方也可以有后续的依赖, 它们在配方之后运行, 用 `&&` 表示:

```
@run: a && b c
  echo run

@a:
  echo a

@b:
  echo b

@c:
  echo c
```

```
$ just run
a
run
b
c
```

依赖之间有相互依赖关系时, 依赖项总是先运行, 即使它们被放在依赖它们的配方之后:

```
a: b c
  echo a

b: c
  echo b

c:
  echo c
```

```
$ just a
c
b
a
```

`just` 命令可以运行多个配方, 当多个配方的规则同配方上的依赖声明规则一致

- 配方参数

配方可以有参数. 这里的配方 `build` 有一个参数叫 `target`:

```
build target:
  @echo 'Building {{target}}…'
  cd {{target}} && make
```

要在命令行上传递参数, 请把它们放在配方名称后面:

```sh
$ just build my-awesome-project
Building my-awesome-project…
cd my-awesome-project && make
```

要向依赖配方传递参数, 请将依赖配方和参数一起放在括号里:

```
default: (build "main")

build target:
  @echo 'Building {{target}}…'
  cd {{target}} && make
```

变量也可以作为参数传递给依赖:

```
target := "main"

_build version:
  @echo 'Building {{version}}…'
  cd {{version}} && make

build: (_build target)
```

命令的参数可以通过将依赖与参数一起放在括号中的方式传递给依赖:

```
build target:
  @echo "Building {{target}}…"

push target: (build target)
  @echo 'Pushing {{target}}…'
```

参数可以有默认值:

```
default := 'all'

test target tests=default:
  @echo 'Testing {{target}}:{{tests}}…'
  ./test --tests {{tests}} {{target}}
```

有默认值的参数可以省略:

```
$ just test server
Testing server:all…
./test --tests all server
```

或者提供:

```
$ just test server unit
Testing server:unit…
./test --tests unit server
```

默认值可以是任意的表达式, 但字符串或路径拼接必须放在括号内:

```
arch := "wasm"

test triple=(arch + "-unknown-unknown") input=(arch / "input.dat"):
  ./test {{triple}}
```

配方的最后一个参数可以是变长的, 在参数名称前用 `+` 或 `*` 表示:

```
backup +FILES:
  scp {{FILES}} me@server.com:
```

以 `+` 为前缀的变长参数接受 一个或多个参数, 并展开为一个包含这些参数的字符串, 以空格分隔:

```
$ just backup FAQ.md GRAMMAR.md
scp FAQ.md GRAMMAR.md me@server.com:
FAQ.md                  100% 1831     1.8KB/s   00:00
GRAMMAR.md              100% 1666     1.6KB/s   00:00
```

以 `*` 为前缀的变长参数接受 0 个或更多 参数,
并展开为一个包含这些参数的字符串, 以空格分隔, 如果没有参数, 则为空字符串:

```
commit MESSAGE *FLAGS:
  git commit {{FLAGS}} -m "{{MESSAGE}}"
```

变长参数可以被分配默认值. 这些参数被命令行上传递的参数所覆盖:

```
test +FLAGS='-q':
  cargo test {{FLAGS}}
```

`{{…}}` 的替换可能需要加引号, 如果它们包含空格. 例如, 如果你有以下配方:

```
search QUERY:
  lynx https://www.google.com/?q={{QUERY}}
```

然后你输入:

```
$ just search "cat toupee"
```

`just` 将运行 `lynx https://www.google.com/?q=cat toupee` 命令,
这将被 `sh` 解析为`lynx`、`https://www.google.com/?q=cat` 和 `toupee`,
而不是原来的 `lynx` 和 `https://www.google.com/?q=cat toupee`.

你可以通过添加引号来解决这个问题:

```
search QUERY:
  lynx 'https://www.google.com/?q={{QUERY}}'
```

以 `$` 为前缀的参数将被作为环境变量导出:

```
foo $bar:
  echo $bar
```

- 位置参数

```
set positional-arguments

@foo bar:
  echo $0
  echo $1
```

将产生以下输出:

```
$ just foo hello
foo
hello
```

`$@`:

```
set positional-arguments

@test *args='':
  bash -c 'while (( "$#" )); do echo - $1; shift; done' -- "$@"
```

```
$ just test foo "bar baz"
- foo
- bar baz
```

- 用其他语言书写配方

以 `#!` 开头的配方被称为 `Shebang` 配方, 它通过将配方主体保存到文件中并运行它来执行. 这让你可以用不同的语言来编写配方:

```
foo:
  #!/usr/bin/env bash
  set -euxo pipefail
  hello='Yo'
  echo "$hello from Bash!"
```

严格意义上说这不是必须的, 但是 `set -euxo pipefail` 开启了一些有用的功能, 使 `bash Shebang` 配方的行为更像正常的、行式的 `just` 配方:

    - `set -e` 使 `bash` 在命令失败时退出.
    - `set -u` 使 `bash` 在变量未定义时退出.
    - `set -x` 使 `bash` 在运行前打印每一行脚本.
    - `set -o pipefail` 使 `bash` 在管道中的一个命令失败时退出. 这是 `bash` 特有的, 所以在普通的行式 `just` 配方中没有开启.

- 配方执行

配方每一行都由一个新的 `Shell` 执行.

多行间设置的变量不能相互使用, 完成一个功能的不能写在多行,
可以转义换行符或者用 `Shebang` 的方式运行

安静执行 `@`:

脚本前添加 `@` 表示命令运行前不打印命令

错误忽略 `-`:

通常情况下, 如果一个命令返回一个非零的退出状态, 将停止执行.  
要想在一个命令之后继续执行, 即使它失败了, 需要在命令前加上 `-`.

[doc](https://github.com/casey/just/blob/master/README.%E4%B8%AD%E6%96%87.md#%E4%BD%8D%E7%BD%AE%E5%8F%82%E6%95%B0)
