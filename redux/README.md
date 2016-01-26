# Redux

## 参考URL

Redux勉強用に簡単なサンプルを作ってみた  
http://qiita.com/DJ_Middle/items/ffa08f983df471a5c6f8  
  
Redux入門【ダイジェスト版】10分で理解するReduxの基礎  
http://qiita.com/kiita312/items/49a1f03445b19cf407b7  
  
reduxを試してみた(5日目) - ajaxを使ってUIを構築する(reduxにおける非同期の制御)  
http://qiita.com/kompiro/items/d1ffcfcba7cc34d364f0  

fluxフレームワークreduxについてドキュメントを読んだメモ  
http://fukajun.org/66

React.js の 公式Tutorial を Redux を利用して書き直した。また Mocha + power-assert を利用したテストも追加
http://qiita.com/ma-tu/items/561cbf84ffeb14dad4a7

## 概要
Reduxは、ReactJSが扱うUIのstate(状態)を管理をするためのフレームワーク。  
Reduxを構成する要素としては下記のような概念がある。
* Action
* ActionCreator
* Store
* State
* Reducer
  
実装ベースでいうと、下記のように落とし込む。
- フレームワークの初期設定をする、エントリーポイント(entry.js)。  
- アプリケーションを描画する Viewコンポーネント群(ルートとなるapp.jsとそれにぶらさがらる子コンポーネント)  
- Viewでユーザが起こすイベントから次に実行するアクションを定義しておく場所 Action Creator(action.js)  
- ActionCreatorから発行されるactionを元にアプリケーションの状態(State)を変更する Reducer(reducer.js)  
- Reducerで変更したアプリケーションの状態(state)を保持する Stores(エントリーポイント内で定義)  
等で構成されます。  

## 実装ベースの構成

http://qiita.com/DJ_Middle/items/ffa08f983df471a5c6f8

上記、最小構成のReduxサンプルを参考に  
どのようにファイルを分割してそれぞれの概念を管理しているかを解説。

#### entry.js

Reduxフレームワークの初期設定をする場所(エントリーポイント)。  
ファイル名としては、bundle.js とかで作られているサンプルも多い。  
  
1. フレームワークのインポート(react,react-dom,redux,react-redux)
2. アプリケーションのルートとなるコンポーネント(app.js)のインポート
3. アプリケーションのStoreの元となるstate情報をReducer(reducer.js)からインポート
4. 3でインポートしたstate情報からアプリケーションのStoreを作成
5. アプリケーションをRenderする要素を定義
6. 2でインポートした、ルートコンポネーントを、
   Providerコンポーネント (react-reduxからインポート) でラップ
7. 6に4のStoreを設定
8. 6,7を行うことで State情報を アプリケーション全体(コンポーネント)に伝搬させる。


#### app.js
Viewを記述するところ。
アプリケーションのルートとなるコンポーネント。

#### action.js
Action Creatorを記述するところ。
Viewでユーザが起こすイベントから次に実行するアクションを定義しておく場所。  
Viewでイベントが起こると、actionを発行して reducerに渡す。

#### reducer.js
action.jsで発行された actionを受け取って、Stateを更新する。

