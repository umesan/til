# npm コマンド

##### モジュールの情報を表示する
```
npm info forever
```


# npm初回公開手順

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



# ユーザを切り替えて公開する
npm に複数ユーザを登録している場合

##### 1. ローカル環境でユーザ切り替え
```
npm adduser
```
上記実行後、
`username`,`password`,`email`
を聞かれるので切り替えたいアカウント情報を入力


##### 3. 公開
公開したいnpmモジュールのあるディレクトリに移動した後、下記を実行
```
npm publish
```


# private modules（@username/project）形式で公開する
private modules（@username/project）形式で、package.jsonを作成し、  
`npm publish` したら、警告が表示された。

```
You need a paid account to perform this action. For more info, visit: https://www.npmjs.com/private-modules
```

有料アカウントにしないと、private modules形式で公開はできないのかと思っていたが、  
よく調べたら無料アカウントでも公開できた。

http://efcl.info/2015/04/30/npm-namespace/

1. package.jsonの nameを `@username/project` にする
2. 公開したいnpmモジュールのあるディレクトリで `npm publish --access=public` を実行

とのこと。
