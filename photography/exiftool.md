# exiftool
写真のEXIFを閲覧・編集できるCLIツール
https://exiftool.org/

## インストール
```sh
brew install exiftool
```

## 使い方
### 閲覧
```sh
exiftool <file_path>
```

### 編集
#### 撮影日時の変更
```sh
exiftool -alldates="YYYY-mm-dd HH:MM:SS" # 撮影日時を特定の日時に変更
exiftool -alldates-="YYYY-mm-dd HH:MM:SS" # EXIFの撮影日時から指定した時間を引く
```
