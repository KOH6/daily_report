## はじめに
[JavaScript Primer - 迷わないための入門書 (jsprimer)](https://jsprimer.net/)の感想を記載いたします。

## 良かったところ
* JSは、暗黙的な型変換についてかなりアバウトであることを学んだ。
* 特にNaNは一度発生すると挙動的にもデバッグ的にもかなり厄介なため、NaNの防止がとりわけ重要であることを学んだ。

## 学んだこと
### 第一部：基本文法
* JavaScriptはECMAScriptの使用によって動作が決められている。ECMAScriptは毎年更新される。
* 変数宣言時に`var`を使用するのは現在非推奨。ECMAScriptでは機能を追加する際にも後方互換性を重視しているため、`var`自体の挙動は変更されなかった。
* REPL(Read-Eval-Print-Loop)
  * コードを評価しその結果を表示する機能のこと。Chromeのデベロッパーツールで叩けるコンソールもその一つ

#### データ型とリテラル
* `typeof null`が`"object"`となるのは、歴史的経緯のある仕様のバグ。
* テンプレートリテラル内で$`{変数名}`と書いた場合に、その変数の値を埋め込むことができる。
```js:テンプレートリテラル
const str = "文字列";
console.log(`これは${str}です`); // => "これは文字列です"
```
* `undefined`はリテラルではない。`undefined`はただのグローバル変数で、`undefined`という値を持っているだけ。
* JSにおいて、ラッパーオブジェクトを明示的に作成する必要はない。ラッパーオブジェクトを使わずとも、ラッパーオブジェクトのプロパティを使用できる。常にリテラルでプリミティブ型のデータを表現すること推奨。

#### 演算子
* NaN
  * 数値ではないがNumber型。
  * NaNはどの値とも（NaN自身に対しても）一致しない特性がある。`Number.isNaN`メソッドを使うことでNaNの判定を行える。
```js:NaNの特性
// 自分自身とも一致しない
console.log(NaN === NaN); // => false
// Number型である
console.log(typeof NaN); // => "number"
// Number.isNaNでNaNかどうかを判定
console.log(Number.isNaN(NaN)); // => true
```
* 厳密等価演算子
  * 意図しない挙動となることがあるため、暗黙的な型変換が行われる等価演算子（==）を使うべきではない。
  * 代わりに、厳密等価演算子（===）を使い、異なる型を比較したい場合は明示的に型を合わせるべき。
  * 例外的に、等価演算子（==）が使われるケースとして、nullとundefinedの比較がある。
```js:例外的に==で比較するパターン
 const value = undefined; /* または null */
// === では2つの値と比較しないといけない
if (value === null || value === undefined) {
    console.log("valueがnullまたはundefinedである場合の処理");
}
// == では null と比較するだけでよい
if (value == null) {
    console.log("valueがnullまたはundefinedである場合の処理");
}
```
* 分割代入
```js:オブジェクトへの分割代入
const obj = {
    "key": "value"
};
// プロパティ名`key`の値を、変数`key`として定義する
const { key } = obj;
console.log(key); // => "value"
```
* falsyな値
  * 以下は全て暗黙的な型変換で`false`に変換される。これらを`falsy`な値という。
    * `false`
    * `undefined`
    * `null`
    * `0`
    * `0n`
    * `NaN`
    * `""`（空文字列）
* Nullish coalescing演算子(`??`)
  * Nullish coalescing演算子(??)は、左辺の値がnullishであるならば、右辺の評価結果を返す。
  * nullishとは、評価結果がnullまたはundefinedとなる値のこと。
  * `0`がfalsyのため`||`では扱えない。この問題を解決するため`??`演算子が導入された。
```js:??演算子の使用例
const inputValue = 任意の値または未定義;

// ||を使った場合
// `inputValue`がfalsyの場合は、`value`には`42`が入る
// `inputValue`が`0`の場合は、`value`に`42`が入ってしまう
const value = inputValue || 42;

// ??を使った場合
// `inputValue`がnullishの場合は、`value`には42が入る
// `inputValue`が`0`の場合は、`value`に`0`が入る
const value = inputValue ?? 42;

console.log(value);
```

#### 暗黙的な型変換
* JSは、型エラーに対して暗黙的な型変換をしてしまうなど、驚くほど曖昧さを許容している。
* そのため、大きなアプリケーションを書く場合は、このような検出しにくいバグを見つけられるように書くことが重要となる。
* 真偽値のtrueが数値の1へと暗黙的に変換されてから加算処理
```js
// 暗黙的な型変換が行われ、数値の加算として計算される
1 + true; // => 2
```
* JSでは、エラーが発生するのではなく、暗黙的な型変換が行われてしまうケースが多くある。
* 暗黙的に変換が行われた場合、プログラムは例外を投げずに処理が進むため、バグの発見が難しくなる。
* 暗黙的な型変換はできる限り避けるべき挙動です。

##### Booleanへの型変換
* 真偽値については、暗黙的な型変換のルールが少ないため、明示的に変換せずに扱われることも多い。

##### Stringへの型変換
* 配列にはjoinメソッド、オブジェクトにはJSON.stringifyメソッドなど、より適切な方法がある。
* そのため、Stringコンストラクタ関数での変換は、あくまでプリミティブ型に対してのみに留めるべき。
* シンボル→Stringへの型変換
  * ES2015で追加されたプリミティブ型であるシンボルは暗黙的に型変換できない。
  * 文字列結合演算子をシンボルに対して利用すると例外TypeErrorを投げるようになっている。
  * そのため、片方が文字列であるからといってプラス演算子の結果は必ず文字列になるとは限らない。
```js:シンボルと文字列の結合
"文字列と" + Symbol("シンボルの説明"); // => TypeError: can't convert symbol to string
```

##### 数値への型変換（主にNaNについて）
* Numberコンストラクタ関数、`Number.parseInt`、`Number.parseFloat`は、 数字以外の文字列を渡すとNaNを返す。
```js:数値への型変換で、NaNとなる例
// 数字ではないため、数値へは変換できない
Number("文字列"); // => NaN
// 未定義の値はNaNになる
Number(undefined); // => NaN
```
* 任意の値から数値へ変換した場合には、NaNになってしまった場合の処理を書く必要がある。
* 変換した結果がNaNであるかは`Number.isNaN(x)`メソッドで判定可能。
```js
const userInput = "任意の文字列";
const num = Number.parseInt(userInput, 10);
if (Number.isNaN(num)) {
    console.log("パースした結果NaNになった", num);
}
```
* NaNは何と演算しても結果はNaNになる特殊な値。次のように、計算の途中で値がNaNになると、最終的な結果もNaNとなる。
```js
const x = 10;
const y = x + NaN;
const z = y + 20;
console.log(x); // => 10
console.log(y); // => NaN
console.log(z); // => NaN
```
* NaNしか持っていない特殊な性質として、自分自身と一致しないというものがある。
* 実際に値がNaNかを判定する際には、`Number.isNaN(x)`メソッドを利用するとよい。
* NaNは暗黙的な型変換の中でももっとも避けたい値となる。
  * 理由として、先ほど紹介したようにNaNは何と演算しても結果がNaNとなってしまうため。
  * これにより、計算していた値がどこでNaNとなったのかがわかりにくく、デバッグが難しくなる。

* 意図しないNaNへの変換を避ける方法として、大きく分けて２つの方法がある。
  * 関数側（呼ばれる側）で、Number型の値以外を受けつけなくする
  * 関数を呼び出す側で、Number型の値のみを渡すようにする
* つまり、呼び出す側または呼び出される側で対処するということ。どちらも行うことによってより安全なコードとなる。
  * 上記の方法を行うためには、定義した関数が数値のみを受けつけるということを明示する必要がある。
* 明示する方法として以下の方法がある。
  * 関数のドキュメント（コメント）として記述する。
  * 引数に数値以外の値がある場合は例外を投げるという処理を追加する。
```js:定義した関数が数値のみを受けつけることを明示する例
/**
 * 数値を合計した値を返します。
 * 1つ以上の数値と共に呼び出す必要があります。
 * @param {...number} values
 * @returns {number}
 **/
function sum(...values) {
    return values.reduce((total, value) => {
        // 値がNumber型ではない場合に、例外を投げる
        if (typeof value !== "number") {
            throw new Error(`${value}はNumber型ではありません`);
        }
        return total + Number(value);
    }, 0);
}
```

##### 明示的な変換でも解決しないこと
* あらゆるケースが明示的な変換で解決できるわけではない。
  * Number型と互換性がない値を数値にしても、`NaN`となってしまう。
  * 一度、`NaN`になってしまうと`Number.isNaN(x)`で判定して処理を終えるしかない。
* JSの型変換は基本的に情報が減る方向へしか変換できない。
* そのため、明示的な変換をする前に、まず変換がそもそも必要なのかを考える必要がある。

##### 空文字列判定について
* 以下のようにBooleanで明示的な型変換をすると、falsyである`0`も空文字列となってしまい意図しない挙動になる。
```js:不十分な空文字判定
// 空文字列かどうかを判定
function isEmptyString(str) {
    // `str`がfalsyな値なら、`isEmptyString`関数は`true`を返す
    return !Boolean(str);
}
```
* 文字列とは「String型で文字長が0の値」であると定義することで、空文字判定の関数をより正確に書くことができる。
* 次のように実装することで、値が空文字列であるかを正しく判定できるようになる。
```js:正確な空文字判定の関数
// 空文字列かどうかを判定
function isEmptyString(str) {
    // String型でlengthが0の値の場合はtrueを返す
    return typeof str === "string" && str.length === 0;
}
console.log(isEmptyString("")); // => true
// falsyな値でも正しく判定できる
console.log(isEmptyString(0)); // => false
console.log(isEmptyString()); // => false
```

## 難しかったこと