## はじめに

React に入門した人のためのもっと React が楽しくなるステップアップコース完全版

## 学んだこと

### セクション 3.再レンダリング最適化

#### memo 機能（コンポーネントのメモ化）

- 子コンポーネントの props 更新時以外に再レンダリングされないようにしたい
  - `memo(props)`として memo 機能を使う。基本的に memo 機能を使ったほうが良い。
  - 複数要素を描画しているコンポーネント、今後肥大化されそうなコンポーネントは`memo`を使う。

#### useCallback 機能（関数のメモ化）

- `const onClickClose = () => setOpen(false);`のようにアロー関数で定義した関数を props として渡すと、毎回新たな関数が定義された渡されていると認識される。
- そのため、`memo(props)`していても子コンポーネントが再レンダリングされてしまう。
  - `useCallback`を使って以下のように記述する。`useEffect`と同様に第二引数に関心を向ける state を指定できる。

```js
// 初期描画時のみに関数生成
const onClickClose = useCallback(() => setOpen(false), []);
// 念のためsetOpenを見張る
const onClickClose = useCallback(() => setOpen(false), [setOpen]);
```

- なぜ`[setOpen]`とするか
  > この関数の関心は「open の state を false にすること」なので、open が今どういう状況かは気にする必要がありません。（[open]とするのは誤り(無意味)なので警告がでてる）
  > ということで、基本的には useCallback で囲った関数内で使用する「変数」「関数」を[]に指定してあげれば正しい動作を保ったまま最適化できるので ESLint でもデフォルトでそのように設定されています。
  > https://www.udemy.com/course/react_stepup/learn/lecture/24823324#questions/14555890

#### 混同しやすい内容

- 変数のメモ化で`useMemo`という機能も存在する。余り使うことはない。変数定義の右辺の中身が複雑化した場合、再レンダリング時に再計算しなくて済むので使用すると良い。

#### セクション 4.様々な CSS の当て方

- 5 種類ほど CSS の当て方が紹介されているので必要に応じて見返す。使用するライブラリによって記法が違う。

#### セクション 5.ルーティング

- React Router を使うのが主流。

#### セクション 6.Atomic Design

- コンポーネントが画面を構成しているという考え方。画面要素を 5 段階に分けてコンポーネント化する。
- React、Vue 用の概念ではない。
- atoms, molocules, organisms, template, pages の 5 要素に分ける。
- AtomicDesign はあくまでベースの概念なので、必要に応じてカスタマイズする。
- 要素の関心を意識してコンポーネントを作ること。
- `grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));`で簡単にレスポンシブに要素の調整ができる。

#### セクション 7.グローバルな state 管理

- グローバルな state がないと、親 → 子 → 孫 → ひ孫 →…という形でバケツリレー式に props を渡していく必要がある。
- Context、Recoil を使うやり方がある。今は Recoil 主流になってそう？

#### セクション 8.JSONPlaceholder でのデータ取得

- JSONPlaceholder のサイトの API を叩くとサンプルの JSON を返してくれる。
- axios で API を叩いて非同期でデータを扱える。

#### セクション 9.TypeScript

- 新規開発では TyoeScirpt を絶対使う！

#### セクション 11.カスタムフック

- コンポーネント内からロジックを分離して別ファイルにしたもの。
- ただの関数。`use`で始まる名前にする。
