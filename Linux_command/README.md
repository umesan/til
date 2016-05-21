# Linuxコマンド


##### dirnameディレクトリを内部のファイルごと削除する
```
rm -rf dirname
```

##### ファイルのリネーム
filename_before から filename_after
```
mv filename_before filename_after
```


##### シンボリックリンク
b  にアクセスすると  a　にアクセスされるようリンク
```
ln -s a b
```

##### シンボリックリンク解除
bに貼ってあるシンボリックリンクを解除
```
unlink b
```
