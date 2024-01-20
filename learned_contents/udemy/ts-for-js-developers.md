## はじめに

JavaScript エンジニアのためのハンズオンで学ぶ TypeScript 徹底入門 2024 年最新版

## 学んだこと

### 基本的な型、感想での型、クラスでの型

- ts ファイルに`export {}`を最初に記述することでモジュール化する。モジュール化することで、変数の名前衝突を解消できる。
- `string[]`以外にも`(boolean | string)[]`のような複数の型を入れた配列の定義も可能（共用型・ユニオン型の定義）
  - タプル型の定義では順番を保証できる。`[boolean, string]`のように書く。
- API で受け取るようなデータに対しては、any 型ではなく interface を自分で定義して型を設定する。
- `never`型は例外を投げる関数につける型。
- `type`は alias をつけるだけ。`typeof`で type からさらに type をつけられる
- 型ガード
  - string か number か戻り値がわからない関数は`unknown`型を設定しておく
  - 戻り地を取り出す際に`typeof hoge === 'number'`というように判定して処理を書く。
  - これを型ガード、タイプガードという。
- `type newType = TypeA & TypeB`と書くことで２つのタイプを混ぜた型定義ができる。インターセクション型という。
- リテラル側の型定義`'日' | '月' ・・・`のように文字列リテラルを制限した型定義も可能。文字列、数値、Boolean でリテラル設定が可能。
- enum の定義も可能。後から定義追加も可能。

```ts
enum COLORS{
  RED : '#FFF000',
  WHITE : '#FFFFFF',
}
let red = COLORS.RED;
let green = COLORS.GREEN; //定義していないのでエラーになる
```

- 引数に`?`をつけることでオプショナルな引数を設定できる。
- オブジェクト指向

  - コンストラクタに戻り値を書いたらコンパイルエラーになる。
  - ts のクラスではゲッターセッターを定義できる。
  - static な関数や変数も定義できる。
  - 抽象クラス抽象メソッドある
  - 多重継承はできない。インターフェースを複数実装することができる。

### 高度な型

- 型の互換性について、オブジェクトの場合は構造のみで判断される。継承関係等は考慮されない。これを構造的部分型という。
- ジェネリクスは以下のように`<>`を使って定義する。慣例的に`T`が使われる。クラスでもジェネリクスを使用できる。

```ts
const echo = <T>(arg: T): T => arg;

console.log(echo<number>(100));
console.log(echo<string>("test"));
console.log(echo<boolean>(true));
```

- `as string`のように as を使うことで any 等の型のアサーションができる。
- オブジェクトのプロパティ変更を阻止したい際には`as const`を使用する。
  - `const`での変数宣言だけでは、プロパティ部分の変更を阻止できない。

```ts
let profile = {
  name: "name",
  height: 178,
} as const; //ここにas constをつける

profile.name = "Ham"; //この変更が不可になる。
```

- `age: number | null`のようにパイプでつなぐことで null 許容した型を定義できる。`nullable`な型という。

- 以下のように index シグネチャを記載することで、指定された型のプロパティなら許容できるようになる。型の柔軟性をあげられる。

```ts
interface Profile {
  name: string;
  underTwenty: boolean;
  [index: string]: string | number | boolean; //indexシグネチャ
}
let profile: Profile = { name: "test", underTwenty: false };
```

### Utilty Types と Conditional Types

- いずれも TS 特有な便利な構文。三項演算子っぽい書き方のものもある。
- **TS に慣れてきてからもう一度見直す**

```ts
// 全部のプロパティをオプショナルにする
type PartialType = Partial<Profile>;
// 全部のプロパティを必須にする
type RequiredType = Required<Profile>;

// MappedType
[P in keyof T]
// のようなin keyofのこと。

// 同様に全部のプロパティをReadOnlyにもできる
type ReadOnlyType = ReadOnly<Profile>;

// TypeAをkeyにTypeBのvalueとした構造を許容する→Recordを使う
Record<TypeA,TypeB >

// Excludeを使えば、ユニオン型から指定の型を除去した型を作れる。
// Extractを使えば、ユニオン型から指定の型を取り出した型を作れる。
// NonNullableを使えば、ユニオン型からnullable以外のDS型を取り出した型を作れる。
```

- Conditional Type は以下のような三項演算子のもののこと。

```ts
type Exclude<T, U> = T extends U ? never : T;
```
