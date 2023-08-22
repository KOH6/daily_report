## はじめに
optparseという表順のライブラリを使用することで、rubyのプログラムにオプションを指定して実行することが可能になります。
本記事ではoptparseの使用方法について簡単にまとめます。

参考記事
[library optparse (Ruby リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/library/optparse.html)

## 実行時のオプションの設定方法
* 例えば-aオプションで指定された引数を受け取りたい場合は以下のように書きます。
```rb:optparseの使用例
# 標準ライブラリのoptparseを読み込む
require 'optparse'

# optparseを使用するためのOptionParserのインスタンス化
opt = OptionParser.new

# オプションの登録
opt.on('-a') { |v| v }

# argumentから引数のみを抽出
opt.parse!(ARGV)
```

* また、`opt.parse(ARGV)`を使用した場合と`opt.parse!(ARGV)`を使用した場合で、`ARGV`の中身が異なります。

### parse!メソッドを使用した場合の出力

```rb:sample.rb
require 'optparse'

opt = OptionParser.new
opt.on('-a') { |v| v }

p ARGV
# 破壊的メソッドの使用
p opt.parse!(ARGV)
p ARGV
```
```sh:parse!メソッド（破壊的）を使用した場合の出力
$ ruby sample.rb -a test
["-a", "test"]
["test"]
["test"] # 破壊的メソッドによりARGVの中身から引数のみを抽出できる
```

### parseメソッドを使用した場合の出力

```rb:sample.rb
require 'optparse'

opt = OptionParser.new
opt.on('-a') { |v| v }

p ARGV
# 非破壊的メソッドの使用
p opt.parse(ARGV)
p ARGV
```
```sh:parseメソッド（非破壊的）を使用した場合の出力
$ ruby sample.rb -a test
["-a", "test"]
["test"]
["-a", "test"] # 破壊的でないためARGVの中身は変わらない
```

## rubyのプログラム内での引数の扱い方
* 上記のような形でトップレベルで引数をparseした場合は、各メソッドの中で`ARGV`を使用して引数を取得できます。例えば、以下のように書いた場合は`ARGV`に格納された0番目の引数を取得してその後の処理を記載できます。

```rb:受け取った引数をメソッド内で使用する例
def hoge
  # ARGVにオプションの引数の値が格納されている。今回は最初の引数の値を使用して条件分岐に使用
  if ARGV[0].zero?
    # 引数が0だった際の処理
  else
    # それ以外の際の処理
  end
end
```

上記をまとめると、以下のような形でメソッドの中でオプションの指定と引数の取得が可能になります。

```rb:sample.rb
require 'optparse'

opt = OptionParser.new
opt.on('-a') { |v| v }

p ARGV
p opt.parse!(ARGV)
p ARGV

def hoge
  if ARGV[0].zero?
    # 引数が0だった際の処理
  else
    # それ以外の際の処理
  end
end
```