# dein.vim

## セットアップ
https://wonwon-eater.com/neovim-susume-plugin-manager/ の記事にあるスクリプトを使う。

```lua
local api = vim.api
local dein_dir = vim.fn.expand('~/.cache/dein')
local dein_repo_dir = dein_dir..'/repos/github.com/Shougo/dein.vim'

vim.o.runtimepath = dein_repo_dir..','..vim.o.runtimepath

-- dein install check
if (vim.fn.isdirectory(dein_repo_dir) == 0) then
  os.execute('git clone https://github.com/Shougo/dein.vim '..dein_repo_dir)
end

-- begin settings
if (vim.fn['dein#load_state'](dein_dir) == 1) then
  local rc_dir = vim.fn.expand('~/.config/nvim')
  local toml = rc_dir..'/dein.toml'
  vim.fn['dein#begin'](dein_dir)
  vim.fn['dein#load_toml'](toml, { lazy = 0 })
  vim.fn['dein#end']()
  vim.fn['dein#save_state']()
end

-- plugin install check
if (vim.fn['dein#check_install']() ~= 0) then
  vim.fn['dein#install']()
end

-- plugin remove check
local removed_plugins = vim.fn['dein#check_clean']()
if vim.fn.len(removed_plugins) > 0 then
  vim.fn.map(removed_plugins, "delete(v:val, 'rf')")
  vim.fn['dein#recache_runtimepath']()
end
```

tomlでプラグインを管理するので、ファイルを作成しておく。
```sh
touch ~/.config/nvim/dein.toml
```

あとはnvimを再起動すればdeinが自動でインストールされる。

## tomlの記述
以下のように[[plugins]]で初めてリポジトリの指定やプラグインの設定を記述する。

```toml
[[plugins]]
repo = 'alpaca-tc/vim-endwise.git'
on_ft = ['ruby']
hook_add = '''
  let g:endwise_no_mappings=1
'''
```
