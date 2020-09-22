# linux 命令行下复制到剪贴板

## 安装

`sudo npm install -g clipboard-cli`

## 修改源码

> 为了能够在 `pipe chain` 中使用

```sh
#!/usr/bin/env node
'use strict';
const meow = require('meow');
const getStdin = require('get-stdin');
const clipboardy = require('clipboardy');

meow(`
    Example
      $ echo 🦄 | clipboard
      $ clipboard
      🦄
`);

function isStdinTTY() {
    return process.stdin.isTTY || process.env.STDIN === '0';
}

function isStdoutTTY() {
    return process.stdout.isTTY || process.env.STDOUT === '1';
}

if (isStdinTTY()) {
    console.log(clipboardy.readSync());
} else {
    (async () => {
        clipboardy.writeSync(await getStdin());
        if (!isStdoutTTY()) {
            console.log(clipboardy.readSync());
        }
    })();
}
```

## 使用

复制文件内容到剪贴板

`cat FILENAME | clipboard`

输出剪贴板内容到标准输出

`clipboard`

输出剪贴板内容到管道

`clipboard | other`

