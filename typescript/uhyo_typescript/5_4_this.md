# 5.4 this
## 5.4.1 関数の中の this は呼び出し方によって決まる
- thisは自分自身を表すオブジェクト
- <object>.<method>の形式で呼び出すと、thisは<object>になる
- <method>を変数に入れるなどしてから呼び出すと、thisは`undefined`になる
- クラスを用いずとも、オブジェクトに対してthisを使った関数を定義できる

```typescript
const user = {
  name: "uhyo",
  age: 26,
  isAdult() {
    return this.age >= 20;
  }
};
```

余談:
同一クラスの異なるインスタンスの持つ関数オブジェクトは同一である。インスタンスごとに異なる挙動を実現するためにthisの仕組みが存在すると言える。

## 5.4.2 アロー関数における this
- アロー関数内のthisは、外側の関数と同じである（自身のthisを持たない）
- 関数内でコールバック関数を使う場合、アロー関数がないときはthisを一時的に変数として定義し直すという必要があったが、アロー関数の登場によってそれが必要なくなった

## 5.4.3 this を操作するメソッド
### applyとcall
- `func.apply(obj, args)`の形で呼び出すことで、「関数funcを、中でのthisをobjにして呼び出す」という意味になる
- argsにはfuncの引数を配列に入れたもの（`func(1, 2, 3)`と呼び出す関数の場合は、`func.apply(obj, [1, 2, 3])`となる）
- callメソッドはapplyと効果は同じだが、呼び出し方が異なる（`func.call(obj, 1, 2, 3)`といったように、引数を1つずつ渡す）

### Reflect.apply
- グローバル変数としてあらかじめ用意されているオブジェクト。メタプログラミングのための機能を持つメソッドがまとまっている
- `func.apply(obj, args)`は`Reflect.apply(func, obj, args)`と書ける
- Reflect.callはない

### bind
- thisが固定された関数オブジェクトを作る関数
- `func.bind(obj)`とすると、func内のthisがobjに固定された関数オブジェクトを得られる

余談:
bind関数には任意の数の引数を渡すことができ、bindが呼ばれた関数の引数を第2引数以降の値で固定した関数を返す（部分適用）
`func(1, 2, 3)`と呼び出す関数に対して`func.bind(obj, arg1, arg2)`とすると、funcの最初の2つの引数がarg1とarg2で固定された関数オブジェクトを得られる

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/bind

## 5.4.4 関数の中以外の this
- トップレベルではthisはundefinedになる
- クラス宣言内では
  - プロパティ宣言の初期値指定の式では、thisはnew時に作られるインスタンスになる
  - 静的プロパティおよび静的初期化ブロックでは、thisはクラスオブジェクトそのものになる
