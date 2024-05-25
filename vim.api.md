# vim.api

## api

### runtime api

- vim.diagnostic

- vim.snippet

- vim.treesitter

- vim.ui

- vim.system

- vim.notify

- vim.notify_once

- vim.lsp.inlay_hint

### inner api

- vim.api.nvim_paste

- vim.api.nvim_put (光标)

### config api

vim.filetype

## hook

- vim.on_key

- vim.paste (与 clipboard 不同, nvim 只是获取文本流不与 clipboard 交互)

- vim.g.clipboard

  - clipboard-osc52

- grepprg/gp (program to use for the `:grep` command default to ripgrep if available)

## util

- vim.iter

- vim.ringbuf

- vim.version

## command

- gx (open the current filepath or URL at cursor using the system default
  handler, by calling `vim.ui.open()`)

## package

- `require('jit.p').start('ri1', '/tmp/profile')`

## option

### flag

- shm

### switch

- termsync: 同步输出减少UI撕裂

> this option helps to avoid all the hit-enter prompts caused by file messages

## default

- `:h commenting`: 内置 comment
