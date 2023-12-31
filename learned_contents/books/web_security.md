# Rails セキュリティガイド - Rails ガイド

## 学んだこと

### セッション

- セッションは、cookie 内のセッション ID によって識別される。
- cookie は Web アプリケーションに一時的な認証機能を提供する。他人の cookie を奪い取ると、そのユーザーの権限で Web アプリケーションを使えるようになる。
- セッションハイジャックの手法
  - ネットワークの盗聴
    - セキュリティに問題のあるネットワークを通過する cookie は盗聴可能。無線 LAN は、まさにそのようなネットワークの一例。
    - 特に暗号化されていない無線 LAN では、接続されているクライアントのすべてのトラフィックを簡単に傍聴可能
    - **対策：SSL による安全な接続の提供**
  - ログアウト忘れ
    - 公共の端末での作業後に cookie を消去するような殊勝なユーザーはほとんどいない。ログアウトするのを忘れたら、次のユーザーはその Web アプリケーションをそのまま使える。
    - **対策：ユーザーには必ずログアウトボタンを提供する。それもよく目立つボタンで。**
  - XSS 攻撃（詳細は後述）
    - クロスサイトスクリプティング（XSS）攻撃は、多くの場合、ユーザーの cookie を手に入れるのが目的。
    - **対策：悪意のある入力をフィルタすることがきわめて重要。同様に、Web アプリケーションの出力をエスケープすることも重要。**
  - セッション固定攻撃（詳細は後述）
    - 攻撃者が自分の知らない cookie をわざわざ盗み取る代わりに、標的の cookie を攻撃者が知っている cookie のセッション ID に固定してしまうという攻撃方法もある。
    - **対策：最も効果的な対応策は、ログイン成功後に古いセッションを無効にし、新しいセッション ID を発行する。**
      - Rails では 1 行のコードで防止できる。

### CSRF について

# 安全なウェブサイトの作り方

## 学んだこと

# CORS 入門

## 学んだこと
