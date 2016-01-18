
# Ruby on Rails

#### 参考URL

小学生でもわかるRuby on Rails入門  
http://openbook4.me/projects/92  
  
Ruby on Rails 4入門 (全28回) - dotinstall  
http://dotinstall.com/lessons/basic_rails_v2  
  
Ruby on Rails チュートリアル  
http://railstutorial.jp/  


## dotinstall 写経

### プロジェクト作成
taskapp という名称でprojectを作成
```
rails new taskapp
```

### サーバ起動
```
rails s
```
### 


### モデルの作成
モデルは単数系で最初の文字が大文字
```
rails generate model Project title:string

// こちらでもOK (generate は g、 stringはデフォルトなので入力不要)
rails g model Project title
```

### モデルを元にDBのスキーマ（枠組み...テーブル構造）を作成
rake db:migrate


### DBの確認
```

// DBの起動
rails db

// DBのスキーマ構造の確認(table名とかがみれる)
.schema

// 終了
.exit


// ruby を使ってプロジェクトやモデルを編集
rails console

// objectの作成
p = Project.new(title:"p1")

// objectを保存、モデルを保存するとDBにも保存される
p.save

// objectの表示
p

// createで new と save をまとめて実行
Project.create(title: "p2");

// Projectを全部表示
Project.all

// console終了
quit

// もう一度DBに入ってDBを見る
rails db

// モデル Projectを保存すると projects テーブルに保存した値が保存される
select * from projects;

// こんな感じの projects テーブルの構造が表示される
1|p1|2016-01-18 14:57:19.408955|2016-01-18 14:57:19.408955
2|p2|2016-01-18 15:00:01.477996|2016-01-18 15:00:01.477996

```

### Controller と View の作成
Controller は複数形
```
rails g controller Projects
```

### URLのルーティングを設定するため、routes ファイルの編集

/config/routes.rb
```
Taskapp::Application.routes.draw do
  resources :projects
end
```

現在のルーティングの確認をする。  
コマンドラインにて下記実行
```
rake routes

// 実行すると下記のような ルーティングの一覧が表示される
-------------------------------------------------------------------
      Prefix Verb   URI Pattern                  Controller#Action
-------------------------------------------------------------------
    projects GET    /projects(.:format)          projects#index
             POST   /projects(.:format)          projects#create
 new_project GET    /projects/new(.:format)      projects#new
edit_project GET    /projects/:id/edit(.:format) projects#edit
     project GET    /projects/:id(.:format)      projects#show
             PUT    /projects/:id(.:format)      projects#update
             DELETE /projects/:id(.:format)      projects#destroy
-------------------------------------------------------------------
一行目で解説すると GET で projects の一覧を取得する処理は、
projectsコントローラーのindexアクションに書くとよいよという意味

```

### Projectsの一覧を表示

controllers に index アクションを記述する。
/app/controllers/projects_controller.rb を編集
```
class ProjectsController < ApplicationController

  // アクションは rubyで index関数を作成
  def index
    // @をつけるとView側で利用できるようになる
    @projects = Project.all
  end

end
```

次に ProjectsController に対する Viewを作成する。
/app/views/projects/index.html.erb を作成編集する。

ProjectsController の index アクションに対するViewの名前は、
/app/views/projects/index.html.erb のように名前が命名規則として決まっている。

```
<h1>Projects</h1>
<ul>
  // ruby の ループ構文
  <% @projects.each do |project| %>
  // <%=  %>で式を評価して表示
  <li><%= project.title %></li>
  <% end %>
</ul>
```

コマンドラインで rails serverを起動して画面確認
```
ruby s
```

ブラウザで下記にアクセス
```
http://0.0.0.0:3000/projects
```





