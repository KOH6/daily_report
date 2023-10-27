## はじめに
ReactアプリをGithub Pagesにデプロイする方法

参考
https://qiita.com/tat_mae084/items/745761eee6cd1d42949d

* `package.json`に以下を追記。
* 注意点
  * docsはroot配下に置くこと。
  * Qiitaのコマンドそのままだと、commit&pushもやっちゃうので個人的に嫌だった。

```json:package.json
{
~ 省略 ~
"homepage":   "homepage": "https://koh6.github.io/react_todo_list/",
~ 省略 ~
  "scripts": {
    ~ 略 ~
    "rm": "rm -rf ../docs",
    "mv": "mv build ../docs",
    "deploy": "npm run rm && npm run build && npm run mv"
  },
}
```