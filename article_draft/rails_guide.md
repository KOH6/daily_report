## はじめに
* 現場で使える Ruby on Rails 5速習実践ガイドを読んでの感想を記載いたします。

## 良かったところ
* Railsの基礎を学べるのが良かったです。Progate等では深く解説されないコントローラや画面描画の裏側の動き等を知ることができ「Railsで開発されたアプリがどうやって動いているか」を学ぶことができました。
* テストの重要性や複数人開発でのノウハウなど、開発全般での学びも多くありました。10章ではコードを記載する上での共通化のテクニック等が記載されており、「実際に開発が進んでいった際にリファクタするためのコツ」が詳細に解説されていました。現場で使える、というタイトルにもあるようにとても実践的な内容に感じました。

## 学んだこと
### 3章
* ジェネレータコマンドの引数を指定する際には、HTTPメソッドがGETになるアクション名を指定すると効率的。HTTPメソッドがGETのアクションは同名のビューを使うことが多いため。
* レンダーは再描画、リダイレクトではリクエストし直す。
* viewで`simple_format`メソッドを使うことで改行やXSS対策を簡単に行える。
* パーシャルにはインスタンス変数を使用せず、ローカル変数として渡すようにする。パーシャルの再利用性、可読性のため。

### 4章
#### マイグレーション
* 1マイグレーションファイルで1バージョンとして扱われる。
* マイグレーションの適用途中でエラーが発生した場合は、その変更はロールバックされる。以降のマイグレーションも適用されない。
* `save`は検証エラー時にfalseを返す。`save!`は検証エラー時に例外を返す。
* `redirect_to`に指定するのは`tasks_path`か`tasks_url`か
  * 使い分けに関しては基本的に_pathでよくて、redirect_toは完全なURLが求められるので_urlを使うようにするよう。[参考](https://qiita.com/Shugo_Y/items/412d7660f259eddf9c66#:~:text=%E4%BD%BF%E3%81%84%E5%88%86%E3%81%91%E3%81%AB%E9%96%A2%E3%81%97%E3%81%A6%E3%81%AF%E5%9F%BA%E6%9C%AC%E7%9A%84%E3%81%AB_path%E3%81%A7%E3%82%88%E3%81%8F%E3%81%A6%E3%80%81redirect_to%E3%81%AF%E5%AE%8C%E5%85%A8%E3%81%AAURL%E3%81%8C%E6%B1%82%E3%82%81%E3%82%89%E3%82%8C%E3%82%8B%E3%81%AE%E3%81%A7_url%E3%82%92%E4%BD%BF%E3%81%86%E3%82%88%E3%81%86%E3%81%AB%E3%81%97%E3%81%BE%E3%81%97%E3%82%87%E3%81%86)
#### 検証
* 検証用メソッドをモデル内に自作することもできる。
* モデルで定義した検証が行われない更新登録操作もあるので注意。
* `before_validation`や`before_save`等のコールバック処理を追加することも可能。
* Railsではコールバック全体で１トランザクションとしてsavaされる。
* テーブル作成時の一意性制約は`t.index :name, unique: true`でユニークインデックスを作る。`uniqueness: true`はモデルのバリデーションの方。
#### ログイン機能
* RailsではCookieでやり取りされるセッションIDをキーとしてセッションが保管される。そのため、ブラウザのCookieを削除するとセッション情報も削除される。
* `has_many :tasks`や`belomgs_to :user`のようなアソシエーションは、`user.tasks`や`task.user`のような便利なRuby上の表記を使用しないのであればテーブル側にカラムを定義するだけでOK。
* モデルに`scope :recent, -> { order(created_at: :desc) }`というようにrecentを定義することで、作成日の降順に並べるソートをカスタムメソッドとして定義できる。

### 5章
#### テストを書くことのメリット
* 環境のバージョンアップやリファクタリングを行うために、テストの存在が必須条件になる。
* テストは仕様を記述したドキュメントとして機能する面もある。Rspecは、動く仕様書としての面に特にフォーカスして作られている。
* テストを作成する過程で仕様バグや要件の詰め漏れ等を発見しやすくなる。
* テストを存在を意識することで、テストの書きやすいプロダクトコードをに自ずとなる。
* Rspecはテストを書くというよりも`Spec`を書く気持ちが大事になってくる。

### 6章
#### ルーティングについて
* routesに書いたURLパターンに対しては、末尾_pathのヘルパーメソッドが自動生成される。
* redirect_to等でURL指定をする際には文字列直接で"/tasks"のように渡すのは推奨されない。ヘルパーメソッドやハッシュで渡す方法を使用するべき。
* ブラウザで利用するアプリケーションの場合は、CRUD以外の独自アクションの追加やリソース階層が深くなる際には、RESTfulを厳密に守る必要はない。
* resources以外にresourceでの定義方法も存在する。
* routes.rb自体も構造化することが可能。scope,namespace,controllerといったメソッドが該当する。
#### セキュリティ対策
* Strong Paramatersでpermitする属性を増やすのを忘れた場合には、エラーが画面に出ないため発見しづらいバグとなる。
* 各種インジェクション（XSS,SQLインジェクション等）はRailsのビューで自動で対策されているものもある。

### 9章
* database.ymlそのものについては、セキュリティ面から通常は構成管理に乗せない。替わりにユーザ名やパスワードを記述しないdatabase.yml.exampleのような形で構成管理して、各マシンでそれをリネームして利用する方法が取られる。

### 10章
* Railsを使い続けるのであればアプリケーションを放棄しない限りずっとバージョンアップをする必要がある。
* bundle updateはチーム全体で行う文化を作るべき。
* 複雑性に対応するための３つの鍵
  * 1.適切な場所にコードを書く。
    * モデルに書くべきコードはモデルに寄せる。
  * 2.上手に共通化する
  * 3.新しい構造を追加して役割を分担する

## 難しかったこと
* Rail5準拠の内容なので最新のRailsバージョンでは非推奨となってる記法のものもある印象を受けました。最新のRailsのバージョンに対応した新版が発売されたらぜひ読んでみたいと思いました。
* アプリを作成する際のビューがslimで記述されているため、文法に慣れるのに苦労しました。ビューについては、erbのままのほうが万人受けしそうな印象を受けました。