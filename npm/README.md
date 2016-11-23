# npm コマンド

##### モジュールの情報を表示する
```
npm info forever
```


# npm公開手順

##### 1. アカウント作成
https://www.npmjs.com/signup


##### 2. ユーザ登録
```
npm adduser
```
上記実行後、
`username`,`password`,`email`
を聞かれるので1で登録したアカウント情報を設定


##### 3. 公開
公開したいnpmモジュールのあるディレクトリに移動した後、下記を実行
```
npm publish
```


##### 4. 公開確認
```
npm info モジュール名
```


##### 5. インストール
```
npm install モジュール名
```




# npm公開をとりやめる
```
npm unpublish パッケージ名前 --force
```
https://ota42y.com/blog/2014/09/29/npm-publish/
