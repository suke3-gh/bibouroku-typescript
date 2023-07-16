# bubouroku-typescript > error-of-event

文書作成日:2023年06月22日 最終更新日:2023年07月17日

`addEventListener(type, listener)`などを用いるとき、`listener`は`Event`という型のオブジェクトを受け取る。以下の例では`obj`の型が`Event`になる。

「Event型の受け取り」
```
target.addEventListener('type', (obj) => {
  obj
})
```

そのためコンパイラオプションによっては受け取ったオブジェクトを`HTMLElement`として扱おうとしてエラーが生じる。

「エラーが発生する記述」
```
HTMLElement.addEventListener('type', (element: HTMLElement) => {
  element.style.display = 'flex'
})
```

ここでは二つのエラーが発生するので、エラーコードごとに対処を記述する。

1. [ts(2769)](#ts2769)
2. [ts(2339)](#ts2339)
    1. [演算子のinstanceofを使用](#演算子のinstanceofを使用)
    2. [型アサーションを使用](#型アサーションを使用)
3. [参考文献](#参考文献)

## ts(2769)
`listener`(この例ではelement)の型は`Event`であるので `HTMLElement`と宣言するのは不適。よって`listener`の型宣言を仕様通りに`Event`とする。ここで`event`ではなく`Event`であることに注意。大文字小文字は区別される。
```
「ts(2769)への対処」
HTMLElement.addEventListener('type', (element: Event) => {
  element
})
```

## ts(2339)
ここでのelementは型が`Event`であるから`HTMLElement`のように`style`などのプロパティを持っていない。つまり、存在していないものを参照していることになるのでエラーが発生する。よって「elementが`HTMLElement`型である」という推論が得られるようにすることでエラーの解決を図る。その手段としては 演算子:`instanceof`を用いる、型アサーションを使用などが存在するようだ。

### 演算子のinstanceofを使用
名称が紛らわしいが、ここでは`element.target`にHTMLのコードが格納されている。`element`だけでは`type`などのプロパティを含んでいるのことに注意。また`element.target`は`EventTarget`という型になっている。そして演算子の`instanceof`と条件式の`if`などを組み合わせ、プロパティの`element.target`が`HTMLElement`型であると明示することで解決する。

「instanceofの使用による解決」
```
HTMLElement.addEventListener('type', (element: Event) => {
  if (element.target instanceof HTMLElement) {
    element.target.style.display = 'flex'
  }
  ...
})
```

### 型アサーションを使用
型アサーションは元々の型がどのようなものであっても、指定した型として扱う事。対象と指定したい型の間に`as`を記述することで使用出来る。ここでは`event.target`が`HTMLElement`であると指定し、その型を格納できる変数を用意して代入する。これによってその変数は型が`HTMLElement`であるので`HTMLElement.style`が使用できるようになる。

「型アサーションの使用による解決」
```
HTMLElement.addEventListener('type', (element: Event) => {
  const html: HTMLElement = event.target as HTMLElement
  html.style.display = 'flex'
  ...
})
```

`instanceof`を用いる手法より型アサーションを使用するのほうがコードの可読性がよく、多くの人が理解しやすいと思われる。しかし内容と型が一致しない状況を作り出すため、他のエラーの原因となることがある。

## 参考文献:
1. https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener (EventTarget: addEventListener() method - Web APIs | MDN)
2. https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style (HTMLElement: style property - Web APIs | MDN)
3. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof (instanceof - JavaScript | MDN)
