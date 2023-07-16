# bibouroku-typescript > about-interface

文書作成日:2023年06月22日　最終更新日:2023年07月17日

`interface`はオブジェクトの型、つまりその内容について宣言する時に用いられる。その使用方法などについて書き留める。

1. [interfaceの宣言](#interfaceの宣言)
2. [interfaceの拡張](#interfaceの拡張)

## interfaceの宣言
型宣言を行いたいオブジェクト名を記述し、 `{}`の中に各値の型を宣言していく。型の宣言の方法自体は変数や関数の場合と変わりない。

「interfaceの宣言」
```
interface name {
  var1: string
  var2: number
}
```

上記の宣言では`name.var1`がstring型、`name.var2`がnumber型と定義される。

## interfaceの拡張
同じ名前で `interface`を再び宣言すると、元々存在していた内容を引き継ぐ。つまり他の言語における`extend`のような挙動となる。

「二つのinterface」
```
interface set {
  var1: string
}

interface set {
  var2: string
}
```

この場合では次のようなinterfaceが宣言された状態になる。

「interfaceの継承」
```
interface set {
  var1: string
	var2: string
}
```
