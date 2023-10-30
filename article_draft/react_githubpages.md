## はじめに

React アプリを Github Pages にデプロイする方法を備忘録としてまとめます。

以下の記事を大いに参考にさせていただきました。
https://qiita.com/tat_mae084/items/745761eee6cd1d42949d

## 前提条件

- Node.js v18.18.0
- npm v9.8.1
- react v18.2.0

## 手順

### アプリのひな形作成

- 以下のコマンドにより React アプリのひな形を作成します。

```sh
$ npx create-react-app my-app
```

### package.json を編集する

- `my-app`配下に`package.json`があるので、以下を追記します。
- 注意点
  - docs ディレクトリは root 配下に置くこと。

```json:package.json
{
~ 省略 ~
//homepageのURLはGithubPagesを公開した際に自動生成されるURLを入力します。
"homepage":   "homepage": "https://<GitHubアカウント名>.github.io/<GitHubリポジトリ名>/",
~ 省略 ~
  "scripts": {
    ~ 略 ~
    "rm": "rm -rf ./docs", //docディレクトリがリポジトリのroot配下になるようにパスを適宜調整してください。
    "mv": "mv build ./docs", //docディレクトリがリポジトリのroot配下になるようにパスを適宜調整してください。
    "github-build": "npm run rm && npm run build && npm run mv"
  },
}
```

### Git に push する

- `docs`の中に build された資材が格納されているので、ブランチに commit します。今回は main ブランチに直接 push する形を想定して記載します。
- この作業が初回の push で、remote に origin を追加していない場合 remote add してから行ってください。

```sh
$ git add .
$ git commit -m 'コミットメッセージ'
$ git push origin main
```

### GitHub Pages の設定を変更する

- GitHub Pages の設定ページから以下を選択してください。
  - [Settings]→[Pages]→[GitHub Pages]→[Build and Deployment]の Branch で以下を選択し Save します。
  - [main], [/docs]
- GitHub Pages の URL が自動生成され、Deploy 完了です。
