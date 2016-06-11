# webpack
>webpack は WebApp に必要なリソースの依存関係を解決し、アセット（配布物）を生成するビルドツール（要するにコンパイラ）です。JavaScript だけでなく、CoffeeScript や TypeScript、CSS 系、画像ファイルなどを扱うことができます。  
>http://qiita.com/yosisa/items/61cfd3ede598e194813b


## webpackでsass

**＜参考URL＞**  
http://qiita.com/nicchi__1985/items/e30e73de6d8443909537

##### 必要パッケージのインストール
package.json に追加して `npm install`
```
  "style-loader": "^0.13.1",
  "css-loader": "^0.23.1",
  "sass-loader": "^3.2.0",
  "extract-text-webpack-plugin": "^1.0.1"
```

後述するトラブルに記載してあるが、`node-sass`も必要
```
"node-sass": "^3.7.0"
```

##### トラブル

###### 1. コンパイルしようとすると ERROR in Cannot find module 'node-sass' が表示される
```
ERROR in Cannot find module 'node-sass'
〜略〜
```
  
`node-sass` がないと怒られるので、package.jsonに追加。


###### 2. コンパイルしようとすると ReferenceError: Promise is not defined が表示される
```
ReferenceError: Promise is not defined
```

nodeのバージョンが古い場合に出るエラー。
node をバージョンアップする。

```
# 現在バージョン
node -v

# npmのキャッシュ削除
npm cache clean -f

# バージョンアップ実行
n stable

# バージョンアップ後のバージョン確認
node -v

```



##### webpack.config.js の設定
下記の感じで設定。
```
/**
 * webpack.config.js
 */
const ExtractTextPlugin = require('extract-text-webpack-plugin');
module.exports = [{
  entry: "./src/sass/app.scss",
  output: {
    path: __dirname + "/src/css",
    filename: "app.css"
  },
  module: {
    loaders: [
      {
        test: /\.scss?$/,
        loader: ExtractTextPlugin.extract("style-loader", "css-loader!sass-loader")
      }
    ]
  },
  plugins:[
    new ExtractTextPlugin("app.css")
  ]
}]
```

