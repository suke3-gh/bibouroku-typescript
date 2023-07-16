# bubouroku-typescript > type-declaration

最終更新日:2023年06月21日 最終更新日:2023年07月17日

TypeScriptでは`let`や`const`のような変数の種類自体はJavascriptと変化はないが、宣言時に型を明示が可能。これの使用例などについて書き留める。

1. [変数での型宣言](#変数での型宣言)
2. [関数での型宣言](#関数での型宣言)
3. [Promiseを使用する場合](#promiseを使用する場合)

## 変数での型宣言
変数の型宣言は変数名の後ろに「`:`」と型の名称を記述することで可能。

「変数の型宣言」
```
const x: number    = 0
const test: string = 'message'
```

## 関数での型宣言
関数については変数の場合と基本的に同様であるが、引数や返り値も型の宣言が可能。返り値の型は関数名の後ろに記述する。

「関数の型宣言」
```
function name(param: number): number {
  return x
}
```

## Promiseを使用する場合
`Promise`を使用するときはその内容の型も明示する必要がある。

「Promiseに対しての型宣言」
```
const promise1: promise<number> = new Promise((resolveFunc, rejectFunc) => {
  func
})
```
