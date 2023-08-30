## はじめに
REST Railsで学んだこと

## 学んだこと
* 序盤はRubyの復習や環境構築の話
* コンテンツがほぼRail7に対応してなさそうなので素直にRails5の方で環境構築したほうが良さそう
* 61
  * boardsテーブルを作成しboardsモデルを作成すれば、インスタンス変数定義しないでも、boardsオブジェクトでカラム名のインスタンス変数を三使用できる。
    * これはO/Rマッパーという機能。DBのテーブルのレコードをプログラムのオブジェクトとして扱う機能
  * railsでは日時を管理していて、db:createした際には未実行日付のmigarationファイルに対して操作しているらしい。
* 62
  * RailsはRESTfulなHTTPメソッドに則ってアクションが作成される。
* 64
  * コントローラ内で定義したインスタンス変数はビューから参照できる。
* 67
  * タイムゾーン設定はapplication.rbに記述する
  * config/initializers配下にフォーマットとして使いたい定数を予め定義しておくこともできる。
* 70
  * ブラウザ上で`localhost:3000/rails/info/routes`にアクセスすると定義済みのパス一覧が表示される
  * `resources :boards`と定義するだけでRESTfulなroutesが自動で設定できる。
* 72
  * partialを使う際はpartialをインスタンス変数ありきの書き方をするのは良くない。親htmlからpartialにインスタンス変数を渡すような書き方推奨
  * partialへの変数の渡し方は3パターンほどあるので、忘れたら動画見直す。
* 74
  * onlyでメソッド名のシンボルの配列を作る際は、%記法ももちろん使える。
* 75
  * seeds.rbと`rake db:seed`コマンドでテストデータを投入できる。
  * ページネーションしたいなら`kaminari`のgemが便利
* 76
  * redirect_toの引数でflashを定義することも可能
* 77
  * `rails-i18n`のgemでエラーメッセージを日本語化できる。
* 78
  * `annotate`というgemにより各モデルのrbのコメントにテーブル定義が記載されるようになる。
* 88
  * 多対多で中間テーブルを介した場合のアソシエーションの書き方
```rb
has_many :board_tag_relations
has_many :boards, through: :board_tag_relations
```
* 89
  * `dependent: :delete_all`を指定すると、destroyメソッドの際に中間テーブルのレコードもまとめて削除してくれる。