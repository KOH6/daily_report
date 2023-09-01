## はじめに
* 現場で使える Ruby on Rails 5速習実践ガイドを読んでの感想を記載いたします。

## 良かったところ
* Railsの基礎が学べる。コントローラや画面描画の裏側の動き等を知ることができる。
* テストの重要性や複数人開発でのノウハウなど、Railsのバージョンに関係ない開発全般での学びが多い、

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
  * 使い分けに関しては基本的に_pathでよくて、redirect_toは完全なURLが求められるので_urlを使うようにするらしい。[参考](https://qiita.com/Shugo_Y/items/412d7660f259eddf9c66#:~:text=%E4%BD%BF%E3%81%84%E5%88%86%E3%81%91%E3%81%AB%E9%96%A2%E3%81%97%E3%81%A6%E3%81%AF%E5%9F%BA%E6%9C%AC%E7%9A%84%E3%81%AB_path%E3%81%A7%E3%82%88%E3%81%8F%E3%81%A6%E3%80%81redirect_to%E3%81%AF%E5%AE%8C%E5%85%A8%E3%81%AAURL%E3%81%8C%E6%B1%82%E3%82%81%E3%82%89%E3%82%8C%E3%82%8B%E3%81%AE%E3%81%A7_url%E3%82%92%E4%BD%BF%E3%81%86%E3%82%88%E3%81%86%E3%81%AB%E3%81%97%E3%81%BE%E3%81%97%E3%82%87%E3%81%86)
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


## 難しかったこと
* Rail5準拠なので最新のRailsバージョンでは適していないものがある。
* ビューがslimで記述されている。erbで良いのでは。