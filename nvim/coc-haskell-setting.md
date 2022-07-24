# Coc Haskellの設定
## ghcupのインストール
https://www.haskell.org/ghcup/

```sh
$ curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

確認

```sh
$ ghci
GHCi, version 8.10.7: https://www.haskell.org/ghc/  :? for help
Prelude> 
```

## Coc設定
.config/nvim/coc-settings.jsonに以下を記述

```json
{
    "languageserver": {
        "haskell": {
            "command": "haskell-language-server-wrapper",
            "args": ["--lsp"],
            "rootPatterns": ["*.cabal", "stack.yaml", "cabal.project", "package.yaml", "hie.yaml"],
            "filetypes": ["haskell", "lhaskell"]
        }
    }
}
```
