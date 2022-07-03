# COC
```toml
[[plugins]]
repo = 'neoclide/coc.nvim'
source = 'release'
build = 'coc#util#install()'
```

### COCのビルド
:call coc#util#install()

## COCプラグイン
### COCプラグインのインストール
dein.tomlのcocのhooks_addで設定していく。

```
hook_add = '''
  let g:coc_global_extensions = [
  \'coc-tsserver',
  \'coc-json',
  \]
  if isdirectory('./node_modules') && isdirectory('./node_modules/prettier')
    let g:coc_global_extensions += ['coc-prettier']
  endif

  if isdirectory('./node_modules') && isdirectory('./node_modules/eslint')
    let g:coc_global_extensions += ['coc-eslint']
  endif
'''
```

### プラグインの設定
:CocConfig

```json
{
    "suggest.triggerAfterInsertEnter": true,
    "suggest.noselect": false,
    "suggest.minTriggerInputLength": 2,
    "suggest.acceptSuggestionOnCommitCharacter": true,
    "suggest.snippetIndicator": " ►",
    "suggest.enablePreview": true,
    "coc.preferences.hoverTarget": "float",
    "coc.preferences.formatOnType": true,
    "coc.preferences.formatOnSaveFiletypes": [
        "typescript",
        "typescriptreact",
        "javascript",
        "javascriptreact"
    ],
    "tsserver.formatOnType": true,
    "eslint.autoFixOnSave": true,
    "eslint.filetypes": [
        "javascript",
        "javascriptreact",
        "typescript",
        "typescriptreact"
    ]
}
```
