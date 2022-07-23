# zinit
## zplugをzinitに置き換え
プラグイン数も大して入っていないので、高速であることを謳っているプラグインマネージャzinitを使ってみる。

https://github.com/zdharma-continuum/zinit

インストール

```sh
$ bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"
$ source ~/.zshrc
$ zinit self-update
```

```
# .zshrc

zinit ice wait'!0'; zinit light "zsh-users/zsh-completions"
zinit ice wait'!0'; zinit light "zsh-users/zsh-autosuggestions"
zinit ice wait'!0'; zinit light "zsh-users/zsh-syntax-highlighting"
zinit ice wait'!0'; zinit light "zdharma/history-search-multi-word"
zinit light "sindresorhus/pure"
```

基本的に遅延読み込み（`zinit ice wait'!0'`）を有効化している。参考: https://qiita.com/9sako6/items/23b164a95b91c644aade

これで計測してみると

```
$ for i in $(seq 1 10); do time zsh -i -c exit; done
zsh -i -c exit  0.52s user 0.18s system 97% cpu 0.720 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.151 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.149 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.152 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.149 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.148 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.150 total
zsh -i -c exit  0.10s user 0.05s system 97% cpu 0.149 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.147 total
zsh -i -c exit  0.10s user 0.05s system 96% cpu 0.148 total
```

かなり高速化できたが初回が遅くなってしまった。
プロファイリングをとってみる。

```
num  calls                time                       self            name
-----------------------------------------------------------------------------------
 1)    1        1085.72  1085.72   95.71%    611.39   611.39   53.89%  compinit
 2)    1         281.17   281.17   24.79%    281.17   281.17   24.79%  compdump
 3)  921         139.55     0.15   12.30%    139.55     0.15   12.30%  compdef
 4)    2          53.61    26.81    4.73%     53.61    26.81    4.73%  compaudit
 5)    1           8.55     8.55    0.75%      8.52     8.52    0.75%  prompt_pure_state_setup
```

compinitとcompdumpで1.2秒かかっている。
プラグインとは別の場所に記述していた`autoload -Uz compinit && compinit`をコメントアウトすると初回に遅くなる問題が発生しない。
そこで、zsh-completionsの読み込みが終わった時点でこのコマンドを実行するようにしてみる。

```
zinit ice wait"!0" atload"autoload -Uz compinit && compinit"
zinit light "zsh-users/zsh-completions"

zinit ice wait"!0" atload"_zsh_autosuggest_start"
zinit light "zsh-users/zsh-autosuggestions"

zinit ice wait'!0'; zinit light "zsh-users/zsh-syntax-highlighting"
zinit ice wait'!0'; zinit light "zdharma/history-search-multi-word"
zinit light "sindresorhus/pure"
```

zsh-autosuggestionsもstartしないとサジェストが出なくなっていたので追記。

これで再度測ってみる。

```
$ for i in $(seq 1 10); do time zsh -i -c exit; done
zsh -i -c exit  0.06s user 0.04s system 70% cpu 0.148 total
zsh -i -c exit  0.06s user 0.03s system 94% cpu 0.094 total
zsh -i -c exit  0.06s user 0.03s system 93% cpu 0.095 total
zsh -i -c exit  0.06s user 0.03s system 93% cpu 0.094 total
zsh -i -c exit  0.06s user 0.03s system 94% cpu 0.097 total
zsh -i -c exit  0.06s user 0.03s system 94% cpu 0.097 total
zsh -i -c exit  0.06s user 0.03s system 94% cpu 0.096 total
zsh -i -c exit  0.06s user 0.03s system 94% cpu 0.097 total
zsh -i -c exit  0.06s user 0.03s system 95% cpu 0.096 total
zsh -i -c exit  0.06s user 0.03s system 95% cpu 0.097 total
```

初回だけ時間がかかるのは変わっていないが、劇的に速くなった。
