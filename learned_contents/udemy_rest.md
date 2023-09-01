## はじめに
REST Udemyで学んだこと

## 学んだこと
### WebAPIとは
* WebAPIとは、Webサービスで提供している機能やデータをプログラムが読み書きしやすい形で使えるよう定義した規約のこと。またはその規約の実装のこと。
* Webサイトは静的コンテンツ。運営者からの情報定期が目的で、ユーザーがサイト内を閲覧して目的が果たされる。
* Webサービスが動的コンテンツ。ユーザーが抱える課題を解決することが目的。ユーザーはサービスに対して情報のインプット・アウトプットを動的に行って目的が果たされる。
* WebAPIの公開
  * WebAPIを公開することで、第三者が公開したAPIで新たな機能を開発してくれる。これにより自サービスの発展につながる。
  * 価値ある機能やデータは全て公開するべき。飲食店の口コミサイトでは、飲食店の検索・予約機能等は価値があるが、郵便番号検索等は価値がない。
  * 公開時のリスクは「入るはずの利益が入らなくなること」でこのリスクはさらに3つのリスクに起因する。
    * 「データが盗まれる」リスクについては、データを活用してもらうため公開しているので対策は不要
    * 以下についてはAPIキーを使った利用者のアクセス制限等で対策できる。
      * 自サービスを使用したサービスが劣っていて、自サービスの評判が落ちて「ユーザが減る」リスク
      * アクセス回数が多いサービスによって「リソースが圧迫される」リスク
* リクエストとレスポンス
  * リクエストはリクエストライン、ヘッダー、ボディの3要素で構成される。
  * リクエストのメソッドで覚えるべきはGET, POST, PUT, DELETEの4種。これらがCRUDに相当するため。
    * POSTはリソース名が決まっていない新規登録。PUTはリソース名が特定できている際の新規登録。
  * レスポンスはステータスライン、ヘッダー、ボディの3要素で構成される。
  * ステータスコード2xxは正常処理、4xxはリクエストに誤りがある、5xxはサーバー処理失敗によるサーバーエラー。
* 安全性と冪等性
  * 副作用とは「リソース（データ）が改変されること」。店舗更新はデータを更新するので副作用がある。店舗情報取得は、データが書き変わらないので副作用がない。
    * 情報取得については、厳密にはアクセスログ出力があるのでリソース変化がある。しかし、データ改変はないので副作用がないと言える。
  * 安全とは「副作用がない読み取り専用であること」。GETやHEADが該当する。
  * 冪等とは「何度実行しても同じ状態が再現されること。副作用の有無は問わない」。副作用があって冪等なケースは、店舗更新で同じIDに同じ店舗名で更新するようなケース。PUTやDELETEが該当する。
  * 安全でも冪等でもないものは、データ登録が該当する。
  * 安全か冪等かをメソッドごとにまとめると以下のようになる。
|メソッド|安全|冪等|
|-|-|-|
|GET|○|○|
|HEAD|○|○|
|POST|-|-|
|PUT|-|○|
|DELETE|-|○|

### RESTについて
* REST(REpresentational State Transfer)：分散型システムにおける設計ルールのこと。
  * 2000年にRoy Fieldingが論文の中で定義した。
* REST原則は6つある。
#### 1.クライアント/サーバー
* クライアントで画面（UI）、サーバーでデータを管理して関心ごとを分離すること。こうすることでクライアントがトリガー、サーバーが受け身となる。
  * ※現在はサーバー側でPUSHも可能なので、RESTが提唱された2000年当初のお話。
#### 2.階層化システム
* 多層アーキテクチャ構成のこと。この構成の中で一つのサーバーのことをコンポーネントと呼ぶ。
* 階層化システムのメリデメ
  * メリット：各システム（各コンポーネント）に役割を決めて独立させることで、コンポーネントの進化と流用ができる。
  * デメリット：データ処理にオーバヘッドが発生するため、ユーザにとって処理が遅くなる。キャッシュを使うことで改善可能。
#### 3.コードオンデマンド
* クライアントコードをダウンロードして実行できること。クライアント側にリリースしたHTMLやJavaアプレットを、リリース後を変更できるのが特徴。
* コードオンデマンドのメリデメ
  * メリット：リリース済みのクライアントに対して機能追加できる。クライアントに処理をさせるため、サーバー側の負荷が下がる。
  * デメリット：クライアントに処理させるため、評価環境が複雑になる。（多数のブラウザ等）
#### 4.統一インターフェース
* 4つの制約が中にある。
* 1.リソースの識別
  * URIを用いてサーバーに保存されたデータを識別する。ここでいうリソースは抽象的な定義も含む。「最新」もリソースなので、URIでlatestを指定して最新情報を取得するのもそのため、
  * =REST原則では「URIはリソースを識別するもの」と定義すること。言い換えると、URIに動作は含まない。
* 2.表現を用いたリソース操作
  * 断面情報（クライアントへ返されるレスポンスやサーバーへPOSTするデータ）を利用してサーバー上のデータを操作する。
* 3.自己記述メッセージ
  * メッセージ内容が何であるか、ヘッダーに記述されている。レスポンスに含まれるメタを見ると内容の概略がわかる。
* 4.HATEOAS(Hypermedia as the Engine of Application State)
  * レスポンスに現在の状態を踏まえて関連するハイパーリンクが含まれる。例）検索結果ページにおける「次のページ」
  * WebAPIではあまり見かけないが、RESTfulにするとHATEOASも含めることになる。
* 統一インターフェースのメリデメ
  * メリット：システムアーキテクチャ全体が簡素化されてわかりやすくなる。サービスの実装に集中でき、独自の進化ができる。異なるブラウザで同じ画面を表示できる。
  * デメリット：インターフェースの標準化により効率が犠牲になる。
#### 5.ステートレス
* 前の状態を保存しないこと。制約として表現すると、サーバーはリクエストだけでコンテキストを理解できること。
* サーバーセッションを使わず、状態はクライアント上に保存することがRESTfulな設計になる。
* ステートレスのメリデメ
  * メリット：リクエストが独立しているので、監視や障害復旧が容易。リクエスト全体でサーバーリソースを共有しないのでスケールが容易。
  * デメリット：単一のリクエストで完結させるため、リクエストデータに重複がある。クライアントの状態が複数バージョンあるのでアプリ制御が複雑になる。
#### 6.キャッシュ制御
* クライアントはレスポンスをキャッシュできる。
* キャッシュを適切に行うことで必要な情報だけリクエストでき、効率的な通信になる。
* キャッシュ制御のメリデメ
  * メリット：ユーザー体験、リソース効率、拡張性の向上
  * デメリット：古いデータを戻すとデータの不整合が起きシステムに対する信頼性低下につながる。

### RESTAPI成熟度モデルについて
* 2010年にMarti Fowlerによりブログで提唱されたもの。LEVEL3はなかなか見かけないので、LEVEL2まで実装しているのが一般的に見かけるRESTAPIに多い。
* LEVEL0：HTTPを使っている
  * 基本レベル。RPCスタイル（Remote Procedure Call　ネットワーク越しに別コンピュータ上のプログラムを実行する仕組み）のXML通信を想定。
* LEVEL1：リソースの概念を導入
  * リソースごとにURLを分割。メソッドはないのでGETかPOSTのみで通信。
* LEVEL2：HTTPの動詞を導入
  * HTTPメソッドによるCRUD操作が行われている。
* LEVEL3：HATEOASの概念を導入
  * レスポンスにリソース感のつながりが含まれる。

### REST WebAPIサービス設計の基本・基本編
* URI設計で考慮すべきこと
  * 冗長にせず短く入力しやすくすること。シンプルで覚えやすいものにして入力ミスを防ぐため。
  * 省略を使わず人間が読んで理解できるものにすること。
  * 大文字小文字を混在させず、すべて小文字にすること。
  * 単語はハイフンでつなげること。歴史的にアンダースコアはタイプライターで下線を引くために存在したので、ハイフンつなぎにする。
    * 単語連結が必要ならそもそもURIを見直す。`example.com/popular-users`なら`example.com/users/popular`にする等
  * 単語は複数形を利用すること。URIで表現するのはリソースの集合。
  * エンコードを必要とする文字を使わないこと。URIから意味が理解できないため、日本語を入れない。
  * サーバー側のアーキテクチャを反映しないこと。悪意あるユーザに脆弱性を突かれる可能性がある。
    * `example.com/cgi-bin/get_user.php`のようにphpファイルの拡張子が見えているとphpで作られているという情報を悪意あるユーザに与えることになる。
  * 改造しやすいこと。`alpha`や`beta`のようなサーバー名の単語をURI上では使わないこと。
  * ルールが統一されていること。`?id=12345`と`/12345/`のようなクエリパラメータとパスパラメータを混在させないこと。
* HTTPメソッドの適用

|操作|API実装例|
|-|-|
|ユーザー情報一覧取得|`GET http://api.example.com/users`|
|ユーザー新規登録|`POST http://api.example.com/users`|
|特定ユーザーの取得|`GET http://api.example.com/users/12345`|
|ユーザーの更新|`PUT http://api.example.com/users/12345`|
|ユーザーの削除|`DELETE http://api.example.com/users/12345`|

* クエリとパスの使い分け
  * クエリパラメータ：`?id=12345`。パラメータが省略可能な場合に使用する。
  * パスパラメータ：`/12345/`。一意なリソースを表すのに必要な場合に使用する。
  * 検索条件等は複数パターンが考えられるので、クエリパラメータを使う。
#### ステータスコード
* 100番台：情報
  * 100 Cntinue：サーバーから拒否されていない
  * 101 Switching Protocol：プロトコルの切り替え
* 200番台：成功
  * 200 OK ：よくみる成功
  * 201 Created：リソース作成成功。本文データに情報がない。ヘッダー情報に新しいリソースのURLが含まれている。
  * 202 Accepted：非同期ジョブの受付成功。処理結果は別途問い合わせてね。
  * 204 No Content：リクエストは成功したが、レスポンスデータがない。つまりクライアント側のビューの変更が不要。
* 300番台：リダイレクト
  * API利用者はリダイレクトを実装しないことが多い。REST APIでは基本的に300番台を使用しない。
* 400番台：クライアントサイドエラー
  * 400 Bad Request：その他のエラー。
  * 401 Unauthorized：認証されていない。
  * 403 Forbidden：リソースアクセス権限がない。
    * このエラーを出すと「存在すること」が確認できてしまう。存在を隠したい場合は、404 Not Foundにする。
  * 404 Not Found：リクエストされたリソースが存在しない。
  * 409 Conflict：リソースが競合して処理が完了できなかった。
  * 429 Too Many Requests：アクセス回数が制限回数（レートリミット）を超えたため処理できなかった。
* 500番台；サーバーサイドエラー
  * 500 Internal Server Error；サーバーサイドのアプリケーションエラー。基本これ。
  * 503 Servixe Unavailable；サービスが一時的に利用できない。メンテナンスや高負荷状態のときに返す。
#### HTTPメソッドとステータスコード
* GETで帰るステータスコードを基準として各メソッドで特徴的なコード
* GET
  * 成功
    * 304 Not Modified：キャッシュ利用して結果を返却する際に使用。GETに関してはキャッシュ利用する際に300番台を返す。
* POST
  * 成功
    * 200は作成したデータをレスポンスに含めたい場合、201は作成後のリソースのURLをヘッダーに入れたいときに返す。
    * 202は、非同期処理で作成を受け付けた際に返す。
  * 失敗
    * 409は、複数端末間で作成データの衝突が起きた際に返す。
* PUT
  * 成功
    * 204はデータ更新で、レスポンスボディにデータを入れない場合に返す。データの更新がない、クライアント側の更新が不要な場合に返す。
  * 失敗
    * 404はデータ更新で該当データがない際に返す。データ登録時には返さない。
* DELETE
  * 成功
    * 削除時はレスポンスにデータを入れないのが普通なので200はあまり使わない。204でできるだけ返す。
  * 削除
    * 404は403を隠すために使用するというニュアンス。
#### データについて
* レスポンスのデータフォーマット
  * XML
    * テキスト形式。タグで記述。タグは入れ子でタグに属性を入れることができる。
  * JSON
    * テキスト形式。JavaScriptをもとにしたフォーマット。**タグがないのでXMLに比べてデータ量が減らせる。**
  * JSONP(JSON with Paddingの略。データがメソッドのPaddingの中に詰め込まれていると覚える。)
    * テキスト形式。実はJavaScriptのソースコード。クロスドメインでデータ受け渡し可能。
* データフォーマットの指定方法
  * URI中で`/users?format=json`クエリパラメータで指定。最もよく使われる。
  * URI中で`/users.json`拡張子で指定。あまり見かけなくなった。
  * リクエストヘッダーで指定。URIがリソースであることを考えると、一番行儀が良い。
* データの内部構造
  * エンベロープ（レスポンスボディ内のメタ情報）は使わない。ヘッダー情報と役割が被り無駄な情報になる。
    * 適切なステータスを返しシンプルな情報にすること。
  * オブジェクトはできるだけフラットにすること。JSONでネストしたオブジェクトを使うとレスポンス容量は増えるため、フラットにして容量を減らす。
  * ページネーションをサポートする情報を返すこと。
    * `"nextPage": 1`のような属性値での情報の返し方はNG。相対的なページの方法では、サーバー側でデータが更新された際にズレが生じうる。
    * `"hasNext": true  "nextPageToken": "aofb639N"`のような次がなにかのキー情報を返すべき。
  * プロパティの命名規則はAPI全体で統一する。混乱を避けるため。
    * キャメルケース：JSONで返すことが多いので、JavaScriptで一般的なキャメルケースを使用することが多い。
    * スネークケース：実はパスカルケースより可読性が高いらしい。
    * パスカルケース：あまり見かけない。
  * 日付はRFC3339(W3C-DTF)を形式を使う。インターネットで標準的に用いられるため。
  * 大きな数値（64bit整数）は文字列で返す。通常の整数は32bit整数で、64bit整数は処理できないため。
#### エラー表現について
* エラー詳細はレスポンスボディに入れること。エラーの詳細コードやエラーメッセージを入れる。
* エラーの際にHTMLが返らないようにすること。レスポンスのフォーマットが変わると、クワイアント側で処理できないケースがある。
* サービス閉塞時は"503" + "Retry-After"。閉塞時に404を返さないこと。再開タイミングまでクライアント側に返すべき。

### REST WebAPIサービス設計・応用編
#### APIバージョンの表現について
* APIバージョンを含めるメリデメ
  * メリット：クライアント側にメリットがある。特定バージョン指定でアクセスできるので突然エラーにならない。
  * デメリット：サーバー側にデメリットがある。複数バージョンを並行稼働させるので、ソースコードやDB管理が複雑になる。
  * 広く世間一般に公開するならメリットをとってバージョンを含めるべき。
* APIバージョンを入れる箇所として、パス、クエリ、ヘッダーの選択肢がある。
  * パスはリソース記述をする箇所なので、設計上はヘッダーに入れるのがきれい。ただ実際はパスに記述することが多い。
  * ヘッダーに入れる際には「X-接頭辞」は今では非推奨なので、サービス固有の接頭辞を入れる。
* セマンティックバージョニング。バージョン1.2.3なら順に`メジャー.マイナー.パッチ`
  * メジャーバージョンアップでは、後方互換しない修正を行う。
  * マイナーバージョンアップでは、後方互換する機能追加を行う。
  * パッチバージョンアップでは。後方互換するバグ修正を行う。
* メジャーバージョンごとにAPIが後方互換しなくなったタイミングでバージョンをつけるのを推奨。管理工数の観点から。

#### OAuthとOpenID Connect
* 認証は「本人特定」、認可は「アクセス制御」。認証してから認可する。
* OAuthもOpenID Connectも認可の仕組みなので、アクセス制御を行うもの。Twitterへの書き込み権限を別サービスに移譲する場合等に使用。
  * OpenID ConnectはOAuthに本人情報取得を加えた仕組み。
* OpenID Connectではサードパーティアプリケーションからサービスに接続する際にアクセストークン以外にIDトークンを使用するのがOAuthとの違い。IDトークンがあるので、サービス側にもう一度渡す際に個人情報を取得することができる。
  * このIDトークンをJSON Web Token(JWT、ジョット)という。

#### JSON Web Token(JWT)
* RFC-7519で使用が標準化されている。
* 特徴は、署名による改ざんチェック、URL-safeなデータ、データの中身はJSON形式の３種。
* 認証結果をサーバーサイドで保存せずクライアントサイドで保持したい際に使用。
* 基本構造はピリオドで区切られた３要素。ヘッダー、ペイロード、署名の３要素。
  * ヘッダー：署名で利用するアルゴリズムなどを定義。JWT固定の"type"項目や、署名時利用するアルゴリズムの項目が格納されている。
  * ペイロード：保存したいデータの実体。セッションに近いイメージなので、セッションに入れるようなデータを入れる。
  * 署名：改ざんされていないかを確認するための署名。
* jwt.ioでJWTを画面上で確認することができる。

#### Authorizationヘッダー
* JWTを使って認証を利用する際にはAuthorizationヘッダーを利用する。
* Authorizationヘッダーのtypeには以下のようなものがある。
  * Basic：ベーシック認証（IDとパスワードを平文で送信）
  * Bearer：OAuth2.0（JWTを使う場合はこれを使用）
  * Digest：ダイジェスト認証（IDとパスワードをハッシュ化して送信）
  * OAuth：OAuth1.0

#### 大量アクセス対策
* 時間あたりのアクセス制限をレートリミットという。
* 観点
  * 誰に対して：APIキー単位とするか、ユーザーID単位とするか等
  * 何に対して：単一機械、機能群、API全体等対象の範囲をどうするか
  * 制限回数：10回、100回、1000回？
  * 単位時間：10分、1時間、1日？制限リセットタイミングの枠をWindowともいう。
* レートリミットの解除をする際はレートリミットアルゴリズムを使う。以下3種がある。
  * Fixed Window：枠（ウィンドウ）ごとにレートリミットが解除される。単位時間切り替え前後にそれぞれMAX回数アクセス許容されるのでいまいち。
  * Sliding Log：単位時間分の過去のログをチェックして判定する。問題点としては、過去ログの大量保存やログ廃棄の問題がある。
  * Sliding Window：Fixed WindowとSliding Logの良いとこ取りをする。前回単位時間のアクセスについても按分して計上するようにする。
* アクセス制限の緩和について
  * 優良顧客の優遇、キャンペーンによる一時的な負荷増大の場合にはアクセス制限緩和するケースがある。
  * そのため、アクセス元ごとに一時的に設定変更できるような仕組みを考慮する必要がある。

#### キャッシュ制御について
* キャッシュさせる方法：キャッシュ制御に利用するヘッダーは2分類3パターン
* Expiresヘッダー
  * キャッシュとしていつまで利用可能かの期限を指定する。
  * Cache-Controlが同時指定されているとExpiresは無視されるので注意。
* Cache-Control + Dateの組み合わせ
  * Cache-Controlでキャッシュの可否や期限を設定。
    * `no-cache`でもクライアント側に保存されるので注意。（逐一サーバー側に使用してよいかお伺いを立てるようになるだけ）通信料が抑えられるので軽量になる。
    * クライアント側で保存したくない場合は、`no-store`で保存不可となる。毎回通信する必要がある。
* Last-Modified + ETagの組み合わせ
  * Last-Modifiedにリソースの最終更新日時を指定する。
  * ETagに特定バージョンを示す文字列を指定する。
* キャッシュさせる単位
  * 応答言語によってキャッシュを分けたい等が考えられるのでURLごとにキャッシュを保存するのでは不十分
  * `Vary`というヘッダーを使うことでキャッシュさせる単位を制御できる。

#### セキュリティ
* APIにもWebサービスと同様にセキュリティ対策が必要
* XSSへの対策：XSS対策用のレスポンスヘッダーを追加
* CSRF
  * 本来拒否しなければならないアクセス元からリクエストを処理してしまう脆弱性
  * 対策：許可しないアクセス元からのリクエスト拒否。攻撃者に推測されにくいトークンを実装。
* HTTP
  * 通信毛糸が暗号化されていないので盗聴されやすい
  * 対策：常時HTTPSを利用した通信にする。
    * SSL：安全に通信するためのプロトコル。2015年に使用禁止。
    * TLS：安全に通信するためのプロトコル。SSLの後継。
    * HTTPS：HTTP + SSL/TLS。
* JWT
  * クライント側で内容の確認・編集が簡単にできてしまう。
  * 対策：ヘッダーのalgに"none"以外を指定して暗号化する。ペイロードのaudに利用者を指定して、受診時に検証するようにする。