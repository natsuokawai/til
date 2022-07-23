# zsh起動高速化
参考資料: https://qiita.com/vintersnow/items/7343b9bf60ea468a4180

## 現状
```
$ for i in $(seq 1 10); do time zsh -i -c exit; done
zsh -i -c exit  0.85s user 0.67s system 107% cpu 1.417 total
zsh -i -c exit  0.82s user 0.62s system 107% cpu 1.339 total
zsh -i -c exit  0.81s user 0.62s system 106% cpu 1.337 total
zsh -i -c exit  0.81s user 0.62s system 105% cpu 1.358 total
zsh -i -c exit  0.82s user 0.62s system 107% cpu 1.338 total
zsh -i -c exit  0.82s user 0.61s system 107% cpu 1.337 total
zsh -i -c exit  0.80s user 0.61s system 106% cpu 1.331 total
zsh -i -c exit  0.81s user 0.63s system 105% cpu 1.357 total
zsh -i -c exit  0.81s user 0.61s system 107% cpu 1.328 total
zsh -i -c exit  0.81s user 0.61s system 107% cpu 1.316 total
```
大体1.3秒強かかっている。

## 素の状態
```
$ : > /tmp/.zshrc
$ time ( ZDOTDIR=/tmp/ zsh -i -c exit; )
( ZDOTDIR=/tmp/ zsh -i -c exit; )  0.01s user 0.01s system 86% cpu 0.022 total
```

## プロファイリング
```
num  calls                time                       self            name
-----------------------------------------------------------------------------------
 1)    1         323.65   323.65   26.52%    323.53   323.53   26.51%  nvm_die_on_prefix
 2)    2         766.58   383.29   62.82%    268.68   134.34   22.02%  nvm
 3)    1         174.10   174.10   14.27%    158.92   158.92   13.02%  nvm_ensure_version_installed
 4)    6         102.02    17.00    8.36%    102.02    17.00    8.36%  compaudit
 5)    1         863.16   863.16   70.74%     96.57    96.57    7.91%  nvm_auto
 6)    3         161.57    53.86   13.24%     59.55    19.85    4.88%  compinit
 7)    1          36.61    36.61    3.00%     18.25    18.25    1.50%  __check__
 8)    4          17.26     4.31    1.41%     17.26     4.31    1.41%  __zplug::sources::github::check
 9)    1          16.70    16.70    1.37%     16.59    16.59    1.36%  __zplug::base::base::git_version
10)    1          53.18    53.18    4.36%     15.19    15.19    1.25%  __zplug::core::core::prepare
```

とりあえず最初の10件。nvm関連が遅そう。compauditが6回呼ばれているのも気になる。

## nvm高速化
以下の記事を参考に、nvm.shを遅延ロードにしてみる。
[nvmやrbenvを遅延ロードさせる \- Runner in the High](https://izumisy.work/entry/2020/05/23/213107)
[How to fix nvm slowing down terminal initialisation \- Growing with the Web](https://www.growingwiththeweb.com/2018/01/slow-nvm-init.html)

.zshrcのnvmに関する記述を以下のうように置き換える。

```sh
if [ -s "$HOME/.nvm/nvm.sh" ]; then
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/bash_completion" ] && . "$NVM_DIR/bash_completion"
  alias nvm='unalias nvm node npm && . "$NVM_DIR"/nvm.sh && nvm'
  alias node='unalias nvm node npm && . "$NVM_DIR"/nvm.sh && node'
  alias npm='unalias nvm node npm && . "$NVM_DIR"/nvm.sh && npm'
fi
```

これにより、nvm、node、npmのいずれかのコマンドを実行する際にnmv.shが実行されるようになる。

```
$ for i in $(seq 1 10); do time zsh -i -c exit; done
zsh -i -c exit  0.23s user 0.21s system 87% cpu 0.500 total
zsh -i -c exit  0.21s user 0.18s system 101% cpu 0.392 total
zsh -i -c exit  0.21s user 0.18s system 100% cpu 0.385 total
zsh -i -c exit  0.22s user 0.19s system 100% cpu 0.399 total
zsh -i -c exit  0.21s user 0.18s system 100% cpu 0.394 total
zsh -i -c exit  0.22s user 0.19s system 100% cpu 0.398 total
zsh -i -c exit  0.21s user 0.19s system 100% cpu 0.398 total
zsh -i -c exit  0.21s user 0.18s system 100% cpu 0.396 total
zsh -i -c exit  0.21s user 0.18s system 100% cpu 0.392 total
zsh -i -c exit  0.21s user 0.18s system 100% cpu 0.393 total
```

nvm.shの遅延ロードだけで0.8秒ほど高速化できた。

rbenvもついでに遅延ロード化。
```
if [ -e "$HOME/.rbenv" ]; then
  export PATH="$HOME/.rbenv/bin:$PATH"
  alias ruby='unalias ruby bundle gem && eval "$(rbenv init -)" && ruby'
  alias bundle='unalias ruby bundle gem && eval "$(rbenv init -)" && bundle'
  alias gem='unalias ruby bundle gem && eval "$(rbenv init -)" && gem'
fi
```

```
$ for i in $(seq 1 10); do time zsh -i -c exit; done
zsh -i -c exit  0.18s user 0.14s system 102% cpu 0.316 total
zsh -i -c exit  0.18s user 0.14s system 103% cpu 0.310 total
zsh -i -c exit  0.18s user 0.14s system 103% cpu 0.307 total
zsh -i -c exit  0.18s user 0.13s system 102% cpu 0.300 total
zsh -i -c exit  0.18s user 0.14s system 103% cpu 0.304 total
zsh -i -c exit  0.18s user 0.14s system 102% cpu 0.306 total
zsh -i -c exit  0.18s user 0.13s system 102% cpu 0.302 total
zsh -i -c exit  0.18s user 0.13s system 102% cpu 0.302 total
zsh -i -c exit  0.18s user 0.14s system 102% cpu 0.303 total
zsh -i -c exit  0.18s user 0.14s system 102% cpu 0.304 total
```

若干速くなった。

これで当初の1.3秒から4倍ほどの高速化を実現できた。

