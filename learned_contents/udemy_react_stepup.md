## はじめに
Reactに入門した人のためのもっとReactが楽しくなるステップアップコース完全版

## 学んだこと
### セクション3.再レンダリング最適化
#### memo機能（コンポーネントのメモ化）
* 子コンポーネントのprops更新時以外に再レンダリングされないようにしたい
  * `memo(props)`としてmemo機能を使う。基本的にmemo機能を使ったほうが良い。
  * 複数要素を描画しているコンポーネント、今後肥大化されそうなコンポーネントは`memo`を使う。
#### useCallback機能（関数のメモ化）
* `const onClickClose = () => setOpen(false);`のようにアロー関数で定義した関数をpropsとして渡すと、毎回新たな関数が定義された渡されていると認識される。
* そのため、`memo(props)`していても子コンポーネントが再レンダリングされてしまう。
  * `useCallback`を使って以下のように記述する。`useEffect`と同様に第二引数に関心を向けるstateを指定できる。
```js
// 初期描画時のみに関数生成
const onClickClose = useCallback(() => setOpen(false), []);
// 念のためsetOpenを見張る
const onClickClose = useCallback(() => setOpen(false), [setOpen]);
```
* なぜ`[setOpen]`とするか
> この関数の関心は「openのstateをfalseにすること」なので、openが今どういう状況かは気にする必要がありません。（[open]とするのは誤り(無意味)なので警告がでてる）
> ということで、基本的にはuseCallbackで囲った関数内で使用する「変数」「関数」を[]に指定してあげれば正しい動作を保ったまま最適化できるのでESLintでもデフォルトでそのように設定されています。
> https://www.udemy.com/course/react_stepup/learn/lecture/24823324#questions/14555890
#### 混同しやすい内容
* 変数のメモ化で`useMemo`という機能も存在する。余り使うことはない。変数定義の右辺の中身が複雑化した場合、再レンダリング時に再計算しなくて済むので使用すると良い。

#### セクション4.様々なCSSの当て方
* 5種類ほどCSSの当て方が紹介されているので必要に応じて見返す。使用するライブラリによって記法が違う。

#### セクション5.ルーティング
* React Routerを使うのが主流。
*
#### セクション6.Atomic Design
* コンポーネントが画面を構成しているという考え方。画面要素を5段階に分けてコンポーネント化する。
* React、Vue用の概念ではない。
* atoms, molocules, organisms, template, pagesの5要素に分ける。
* AtomicDesignはあくまでベースの概念なので、必要に応じてカスタマイズする。
* 要素の関心を意識してコンポーネントを作ること。
