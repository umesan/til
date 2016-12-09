# Yarn

>Yarnはnpmと互換性のあるFacebookが作ったJavaScriptのための新しいパッケージマネージャー。
>npmに対して「インストールが高速」「より厳密にバージョンを固定」「セキュリティが高い」といった特徴がある。
>
>https://ics.media/entry/13838


# 公式
https://yarnpkg.com/


# install
```
brew update
brew install yarn
```

or

```
npm install -g yarn
```

**インストール後のバージョン確認**
```
yarn --version
```

# パッケージのインストール
```
yarn
```


# scriptsの実行
```
yarn start
yarn run hoge
```


# package.json
互換性あるため、npm と特に変わらず。



# npm からの移行で覚えておいたほうがよいこと

|     npm     |     yarn     |
|-------------|--------------|
| npm install | yarn         |
| npm start   | yarn start   |
| npm run xxx | yarn run xxx |

**その他**  
https://yarnpkg.com/en/docs/migrating-from-npm






