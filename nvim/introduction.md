# Introduction
https://neovim.io/

基本的にNeovimのすゝめという連載記事を参考にしている。
https://wonwon-eater.com/category/programing/neovim-susume/

## 設定ファイル
$HOME/.config/nvim/init.lua

vim同様vim scriptでも書けるが、nvim 0.5以降はLua推奨とのこと。

Luaの解説記事
https://github.com/willelz/nvim-lua-guide-ja/blob/master/README.ja.md

`:luafile %`でinit.luaを再読込できる。

## 基本設定
```lua
vim.o.hidden = true
vim.o.ignorecase = true
vim.o.smartcase = true
vim.o.updatetime = 300
vim.bo.expandtab = true
vim.bo.autoindent = true
vim.bo.smartindent = true
vim.bo.tabstop = 2
vim.bo.shiftwidth = 2
vim.bo.autoread = true
vim.wo.cursorline = true
vim.wo.number = true
vim.cmd 'lan en_US.UTF-8'
vim.cmd 'set clipboard+=unnamedplus'
```

言語設定のUTF-8をつけないと日本語がyankできなくなる。

## キーマッピング
vim.g.mapleader = ' '
api.nvim_set_keymap('n', '<C-N>', ':bnext<CR>', { noremap = true })
api.nvim_set_keymap('n', '<C-P>', ':bnext<CR>', { noremap = true })

## 操作
```
:e <filename> // ファイルを開く
:ls // バッファ一覧を表示
:bn // N個隣のバッファを開く
:bN // N番目のバッファを開く
:terminal // ターミナルを開く
<C-\><C-n> // terminalのインサートモードを抜ける
```
